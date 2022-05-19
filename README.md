# uploadjsonfiletoawsamplify
Steps to upload a json file on your iOS app to AWS using AWS Amplify(Step by Step Guide)

As of May 2021 - XCode continuously updates so things that work previously may not work currently

I had faced difficulties uploading to aws on Xcode and found out that using AWS Amplify is the best (easiest) way

Assuming you already have your Xcode Project on hand

# On your Mac terminal, enter the codes into your terminal to setup your workspace
```

cd Desktop/youriosproject/ 

//assuming your project is saved on Desktop

pod init

ls -la

sudo nano Podfile

//this code will ask you for your password to your computer

//after that it will open up a podfile

//type these codes in your podfile under #Pods for youriosProject (will upload my podfile)

pod 'AWSCore'

pod 'AWSS3'

pod 'AWSCognitoIdentityProvider'

//select Exit by clicking "Ctrl x", select Yes by entering "y", then press Enter

pod install

//wait for it to finish running and your workspace project file for your project will appear in the file


<img width="633" alt="Screenshot 2022-05-11 at 5 14 24 PM" src="https://user-images.githubusercontent.com/100278023/167814707-e1902708-7c03-4027-b1e0-909fe38d80fc.png">

//you can quit the terminal

----------------------------------------------------

# Open your terminal again

amplify configure

//amplify configure will lead you to login into your AWS account

cd Desktop/youriosproject/

amplify init

amplify add storage

//questions will appear on the console so just answer accordingly

amplify push

---------------------------
# moving onto your workspace project file

//add Amplify to your xcode project


Right clight on your project (with the blue icon beside)


Add packages
![Screenshot 2022-05-11 at 5 47 30 PM](https://user-images.githubusercontent.com/100278023/167821651-c001547d-5716-432b-b111-7c7aa4a81f76.png)


Enter package URL: https://github.com/aws-amplify/amplify-ios

Select the Amplify, AWSCognitoAuthPlugin, and AWSSS3StoragePlugin package products.

------------------
# in your AppDelegate file

import UIKit

import Amplify

import AWSCognitoAuthPlugin

import AWSS3StoragePlugin



@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {
    var window: UIWindow?
    
    
    override init() {
        super.init()
        self.configureAmplify()
        }
        
    func configureAmplify() {
            do {
                try Amplify.add(plugin: AWSCognitoAuthPlugin())
                try Amplify.add(plugin: AWSS3StoragePlugin())
                
                try Amplify.configure()
                print("Successfully configured Amplify")
                
            } catch {
                print("Could not configure Amplify", error)
            }
        }
}

------------------------
# in your ViewController

//convert your data into json data and print to a json file
func getjsonfile() {

        let encodedDictionary = try? JSONEncoder().encode(thedict)
     
        
         if let documentDirectory = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first {
             let pathWithFileName = documentDirectory.appendingPathComponent("yourdata.json")
             do {
                 try encodedDictionary?.write(to: pathWithFileName)
            
                 uploadjsonfile(jsonfileurl: pathWithFileName)
             }
             
             catch {
                 print("fail to print to file")
             }
            
     }
    }
    
    //upload the json file
     func uploadjsonfile(jsonfileurl: URL) {
        
        
        Amplify.Storage.uploadFile(key: "hey", local: jsonfileurl, options: nil) { result in
            switch result {
            case .success(let uploadedData):
                print("upload success!")
              //  print(uploadedData)
            case .failure(let error):
                print(error)
            }
        }
    
    }

//the string you input for key will be the name of the file uploaded onto the AWS bucket

//remember to call getjsonfile() somewhere in your code in ViewController
```
----------------
# Things to note if your program fails to build and run
Click on your Project (with the blue icon on the side)

Click on Targets

Under "[CP] Embed Pods Frameworks", delete items under the Output File/Output File Lists

Click on Project

Under Info

Set Debug and Release to None (Ususally it is auto-set to SampleCode)

----------------

I got a lot of help from

1. Setting up aws amplify

(Youtube Tutorial) https://www.youtube.com/watch?v=4k2zNLIkHYs

(Article Tutorial) https://www.kiloloco.com/articles/011-uploading-files-from-ios-to-amazon-s3/

2. Setting up workspace and pod installation (the upload to aws part for this tutorial no longer works as some codes have been deprecated)

(Youtube Tutorial) https://www.youtube.com/watch?v=INg_-DCDq9U&t=296s

-----------------
I have tried to detail all the steps that I have used and had worked for my project

Hopefully this ReadMe file will be useful for you, thank you for reading :)

