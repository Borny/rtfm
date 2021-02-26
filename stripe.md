# Stripe

Online payment

- Angular
- NodeJs

## Angular 

- Typing
- Import Stripe.js
- Import elements
- Stripe cards

### Typing

Install the Stripe typings to get autocomplete from the IDE   
`npm install --D @types/stripe-checkout`
`npm install --D @types/stripe-v3`

Add the following to the **tsconfig.app.json** file in the **type** array of the compilerOptions object:  
`"stripe-v3"`  

### Import Stripe.js

In the **index.html** import stripe in the *head* tag:  
`<script src="https://js.stripe.com/v3/"></script>`  

### Create the card element 



### Stripe cards
**French**  
Number: 4000002500000003	
Token: tok_fr	 
Payment Method: pm_card_fr   

## NodeJs

Install the **stripe** package:  
`npm i --save stripe`   
