##1.简述
    在我们使用聊天工具进行交流的时候，我们会看到当键盘弹出后，聊天界面向上平移的动画，
    让用户输入信息的时候能够看到自己输入的信息，使键盘无法遮盖文本框。

##2.使用的方法
    在这里我们有两种方法可供选择:
    2.1 UITextFieldDelegate中的
    - (void)textFieldDidBeginEditing:(UITextField *)textField; 
    - (void)textFieldDidEndEditing:(UITextField *)textField;   
    通过这两个方法来获取键盘是否弹出的状态，然后对视图进行向上平移的动画

    2.2 使用NSNotificationCenter
    使用消息通知中心，来检测键盘是否弹出的状态，同时，苹果官方也提供了监视键盘状态的接口：
![键盘状态监视接口属性](http://upload-images.jianshu.io/upload_images/2061725-a89c7e327e4fa347.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    2.3 在这里，我推荐大家使用NSNotificationCenter来获取键盘的状态，
    因为，在后面我们还需要得到键盘的尺寸大小，来确定我们的视图应该向上平移多少像素，
    同时，苹果自带的键盘有多个类型，高度不一，
    我们需要通过访问弹出的键盘的CGSize来控制平移的像素到底是多少,
    我们直接可以通过KVO方法获取到，而不需要自己判断是哪一种键盘，然后获取它的尺寸。
     NSDictionary* info = [aNotification userInfo];
    // 获取到键盘尺寸
     CGSize keyboardSize = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue].size;

##3.代码
    3.1 首先添加消息通知机制
    - (void)addKeyboardNotifications {
        //使用NSNotificationCenter 键盘将要出现时
        [[NSNotificationCenter defaultCenter] addObserver:self
                                                 selector:@selector(keyboardWillShow:)
                                                     name:UIKeyboardWillShowNotification 
                                                   object:nil];

        //使用NSNotificationCenter 键盘将要隐藏时
        [[NSNotificationCenter defaultCenter] addObserver:self
                                                 selector:@selector(keyboardWillBeHidden:)
                                                     name:UIKeyboardWillHideNotification 
                                                   object:nil];
    }

----
    3.2 消息通知机制有添加，就必须有删除，不然它会一直运行
    在这里我选择在退出该界面时移除
    - (void)viewWillDisappear:(BOOL)animated {
        // 离开该界面，自动移除消息通知机制
        [[NSNotificationCenter defaultCenter] removeObserver:self];
    }
----
    3.3 当想用的机制被触发时，调用下面的函数
    // 实现当键盘将出现的时候计算键盘的高度大小, 用于向上平移视图显示输入框
    - (void)keyboardWillShow:(NSNotification*)aNotification {
        NSDictionary* info = [aNotification userInfo];
        // 获取到键盘尺寸
        CGSize keyboardSize = [[info objectForKey:UIKeyboardFrameEndUserInfoKey] CGRectValue].size;
    
        //视图向上平移动画加载
        [UIView animateWithDuration:0.3 animations:^{
            CGRect frame = self.view.frame;
            frame.origin.y = -keyboardSize.height;
            self.view.frame = frame;
        }];
    }

    //当键盘隐藏的时候
    - (void)keyboardWillBeHidden:(NSNotification*)aNotification {
        [UIView animateWithDuration:0.3 animations:^{
            CGRect frame = self.view.frame;
            frame.origin.y = 0.0;
            self.view.frame = frame;
        }];
    }
