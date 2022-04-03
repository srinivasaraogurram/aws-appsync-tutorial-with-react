https://medium.com/@benawad/aws-appsync-tutorial-with-react-4e272a6f3527

Error/Trouble Shooting:
https://ginnyfahs.medium.com/github-error-authentication-failed-from-command-line-3a545bfd0ca8

git push origin master

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
Step1 : 

1.1: Start by installing AWS Amplify CLI:
        npm i -g @aws-amplify/cli
1.2: Next configure it with your AWS account:
        amplify configure
1.3: Create a React project:
        npx create-react-app my-app --scripts-version=react-scripts-ts
Note:   I’m choosing to use the Typescript version

1.4: Add Amplify to the project:
        amplify init

1.4.1: Choose the type of app that you’re building: javascript
1.4.2: What javascript framework are you using: react
1.4.3: I kept the default for the rest of the questions and said Yes to use an AWS Profile.
1.4.4: Now that we have Amplify initialized in our project, we can have it create a GraphQL API for us:
        amplify add api
1.4.4.1: Please select from one of the below mentioned services: GraphQL
1.4.4.2: Choose an authorization type for the API: API Key
1.4.4.3: Do you have an annotated GraphQL schema? N
1.4.4.4: Do you want a guided schema creation? Y
1.4.4.5: What best describes your project: One-to-many relationship (e.g., “Blogs” with “Posts” and “Comments”)
1.4.4.6: Do you want to edit the schema now? Y
            user notes: 
                        It should now show you the sample GraphQL schema it setup for you. 
                        You can play with the fields if you like 
                        and then hit enter in the command line when your done.
                        
1.4.4.7: To have AWS generate the GraphQL API we have to push up our changes:
            amplify push
                user notes: We can also have it generate some code for us:

1.4.4.7.1: Do you want to generate code for your newly created GraphQL API: Y
1.4.4.7.2: Choose the code generation language target: typescript
1.4.4.7.3: Do you want to generate/update all possible GraphQL operations — queries, mutations and subscriptions: Y
The other questions, I kept the defaults.
                user notes:
                        When it’s done creating all that stuff, you can go to the AppSync dashboard on AWS
                        and see your API. You can also interact with it in the editor they provide.
1.4.4.8: Calling a Mutation from React
First configure Amplify by adding the following code to src/index.tsx :

import Amplify from '@aws-amplify/core'
import config from './aws-exports'
Amplify.configure(config)

That gives me some tslint warnings, so I changed my tslint.json to look like this:
```
{
"extends": ["tslint:recommended", "tslint-react", "tslint-config-prettier"],
"jsRules": {
"object-literal-sort-keys": false
},
"rules": {
"member-access": false,
"object-literal-sort-keys": false,
"ordered-imports": false,
"interface-name": false,
"no-submodule-imports": false,
"no-implicit-dependencies": false,
"no-empty": false,
"jsx-no-lambda": false,
"no-console": false
},
"linterOptions": {
"exclude": [
"config/**/*.js",
"node_modules/**/*.ts",
"coverage/lcov-report/*.js"
]
}
}
```
We also need to install some packages:
yarn add aws-amplify aws-amplify-react
To make sure everything is working so far, you can run yarn start to see the website and it should display a screen that says Welcome to React.
Let’s add src/Form.tsx which we will use to type in the name to create a blog:
import * as React from "react";
interface State {
name: string;
}
interface Props {
onSubmit: (formValues: State) => void;
}
export class Form extends React.PureComponent<Props, State> {
state = {
name: ""
};
handleChange = (e: any) => {
const { name, value } = e.target;
this.setState({ [name]: value } as any);
};
render() {
return (
<form
onSubmit={async e => {
e.preventDefault();
this.props.onSubmit(this.state);
}}
>
<h3>Create Blog</h3>
<input
name="name"
placeholder="name"
value={this.state.name}
onChange={this.handleChange}
/>
<button type="submit">save</button>
</form>
);
}
                        
