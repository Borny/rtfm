# Deploying a projet to Firebase hosting

Install the firebase CLI:  
`npm install -g firebase-tools`  

## Using Angular

- Build the angular project
- Authenticate to firebase
- Init the firebase project
- Deploy the project 

### Build the Angular project

`ng build --prod`  
Will create a **dist/** folder (or **www** when using Ionic).

### Authenticate to firebase
`firebase login`

### Init the firebase project
Create a project in the firebase console.
Then run   
`firebase init`  
Choose the **hosting** option  
Enter the name of the public directory (i.e: **dist** or **www**)  
Enter **yes** to the configure as a single page app option  
Enter **no** to the overwrite index.html file option

### Deploy the project 
Run  
`firebase deploy`
