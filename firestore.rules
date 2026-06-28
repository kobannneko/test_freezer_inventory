rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    function isAllowedUser() {
      return request.auth != null && (request.auth.token.email == "kobannneko@gmail.com" || request.auth.token.email == "mi.4645.na@gmail.com");
    }
    match /{document=**} { allow read, write: if isAllowedUser(); }
  }
}
