# test
// Use the Firebase /Auth and /Database
//
//


import UIKit
import Firebase
import FirebaseAuth
import FirebaseDatabase



class ViewController: UIViewController {

    var ref: DatabaseReference?
    
    @IBOutlet weak var emailField: UITextField!
    @IBOutlet weak var passwordField: UITextField!
    
    @IBAction func didLogin(_ sender: UIButton) {
        let email = self.emailField.text!
        let password = self.passwordField.text!
        
        Auth.auth().signIn(withEmail: email, password: password) { (user, error) in
                if let error = error {
                    print(error.localizedDescription)
                    return
                }
                print("logged in succesfully")
            
            let currentDate = Date().description(with: Locale.current)
            
            self.getRef().child("users").child(user!.uid)
                .setValue(["loggedInAt" : currentDate])
            
            }
    
    }
    
    @IBAction func didRegister(_ sender: UIButton) {

        let email = self.emailField.text!
        let password = self.passwordField.text!
        
        Auth.auth().createUser(withEmail: email, password: password) { (user, error) in
            if let error = error {
                print(error.localizedDescription)
                return
            }
            print("\(user!.email!) created")
        }
    }

    func getRef() -> DatabaseReference {
        if self.ref == nil {
            self.ref = Database.database().reference()
        }
        return self.ref!
    }
}


