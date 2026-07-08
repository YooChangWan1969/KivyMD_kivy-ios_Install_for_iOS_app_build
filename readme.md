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
