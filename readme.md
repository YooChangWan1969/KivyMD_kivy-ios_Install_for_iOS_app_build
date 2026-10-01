# Review This Page
https://nrodrig1.medium.com/put-kivy-application-on-iphone-b9b4fd4692e9

1. The First INSTALL Python == 3.11.9

2. THE SETTING for kivy-ios

3. Xcode app required Install

# THE command

A. cd python_env_directory

B. git clone https://github.com/kivy/kivy-ios.git

C. cd kivy-ios

D. source ../bin/activate

E. pip install -e .                                                                                                      # It's a "." 

F. toolchain build kivy python3 pillow

G. toolchain pip install kivymd==1.2.0

H. toolchain pip install beautifulsoup4==4.12.2

I. toolchain create Developing_directory /Users/MyName/python_env_directory/Developing_directory                         # It's your working of Developing_directory

J. toolchain icon Developing_directory-ios /Users/MyName/python_env_directory/Developing_directory/images/favicon.png    # favicon.png .... It's icon image file

K. open Developing_directory-ios/Developing_directory.xcodeproj/


# Append : Xcode 27 + iOS 27 SDK

1. Create MySceneDelegate.m and Move Xcode's Classes session

<pre>
#import 
#import 

// Kivy/SDL이 만드는 UIWindow를 연결할 Scene
static UIWindowScene *gKivyScene = nil;

#pragma mark - UIWindow: 생성 시점에 Scene 자동 지정

@interface UIWindow (KivyScenePatch)
@end

@implementation UIWindow (KivyScenePatch)

+ (void)load {
    SEL origSel = @selector(initWithFrame:);
    SEL swzSel  = @selector(kivy_initWithFrame:);
    Method origM = class_getInstanceMethod(self, origSel);
    Method swzM  = class_getInstanceMethod(self, swzSel);

    if (class_addMethod(self, origSel,
                        method_getImplementation(swzM),
                        method_getTypeEncoding(swzM))) {
        class_replaceMethod(self, swzSel,
                            method_getImplementation(origM),
                            method_getTypeEncoding(origM));
    } else {
        method_exchangeImplementations(origM, swzM);
    }
}

- (instancetype)kivy_initWithFrame:(CGRect)frame {
    // Since the methods have been swapped, this call executes the original initWithFrame: implementation.
    UIWindow *w = [self kivy_initWithFrame:frame];
    if (w && gKivyScene && w.windowScene == nil) {
        w.windowScene = gKivyScene;
    }
    return w;
}

@end
</pre>

2. Patch main.m : Add a category to forward connection routines before the core UIApplicationMain runtime sequence triggers

<pre>
@interface NSObject (KivyIOS27ScenePatch)
- (UISceneConfiguration *)application:(UIApplication *)application configurationForConnectingSceneSession:(UISceneSession *)connectingSceneSession options:(UISceneConnectionOptions *)options;
@end

@implementation NSObject (KivyIOS27ScenePatch)
- (UISceneConfiguration *)application:(UIApplication *)application configurationForConnectingSceneSession:(UISceneSession *)connectingSceneSession options:(UISceneConnectionOptions *)options {
    UISceneConfiguration *config = [[UISceneConfiguration alloc] initWithName:@"Default Configuration" sessionRole:connectingSceneSession.role];
    config.delegateClass = NSClassFromString(@"MySceneDelegate");
    return config;
}
@end
</pre>

3. Append AppName-Info.plist Entry with <dict></dict>

<pre>
	
<key>UIApplicationSceneManifest</key>
<dict>
	<key>UIApplicationSupportsMultipleScenes</key>
	<false/>
	<key>UISceneConfigurations</key>
	<dict>
		<key>UIWindowSceneSessionRoleApplication</key>
		<array>
			<dict>
				<key>UISceneConfigurationName</key>
				<string>Default Configuration</string>
				<key>UISceneDelegateClassName</key>
				<string>MySceneDelegate</string>
			</dict>
		</array>
	</dict>
</dict>
		
</pre>
