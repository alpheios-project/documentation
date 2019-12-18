## Alpheios Development Testing Practices

### Goals

1. Verify business requirements are met
2. Provide documentation of how the code is expected to behave
3. Catch errors introduced by bug fixes and new features 
4. Verify edge cases and handling of failure conditions
5. Confirm currect functioning of business logic when confronted with data
6. Verify API contracts are upheld
7. Make sure all the parts work together
8. Identify needed architectural improvements
9. Encourage the development of debuggable code

### Codebase categories

1. data models
2. business logic components
3. UI components
4. encapsulated feature sets
5. service clients
6. microservices 
7. lambda functions
8. application packages
9. infrastructure/deployment code 

### Practices

1. All codebases should use linters to enforce coding standards
2. All codebases should include unit tests which cover the unique functionality of the underlying code without any package dependencies
3. Unit tests should run with every push to GitHub
4. All codebases should include integration tests which cover the behavior of the underlying code with package dependencies 
5. Integration and Unit tests should avoid dependencies on any production micro services 
6. Application pages should include end-to-end tests which may have dependencies on production microservices
