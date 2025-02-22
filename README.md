# Person Filtering Utility

## Overview 

This TypeScript utility provides a flexible function, filterPersons, to filter a list of persons based on specific criteria. The function supports filtering both User and Admin types while maintaining type safety.


## Features
1. Supports filtering User and Admin entities separately.

2. Allows partial matching on attributes (excluding type).

3. Ensures type safety through TypeScript's conditional types.

4. Handles array-based criteria matching 

## Installation

To use this utility in your project, follow these steps:

     npm install typescript --save-dev

Ensure TypeScript is set up in your project.


## Interface Definitions

     interface User {
          type: 'user';
          name: string;
          age: number;
           occupation: string;
      }


     interface Admin {
         type: 'admin';
         name: string;
         age: number;
         role: string;
     }


     export type Person = User | Admin;




## Function Definiton


        export function filterPersons(
              persons: Person[],
              personType: 'user' | 'admin', 
              criteria:  Partial<Omit<Person, 'type'>>
         ): Person[] {
               return persons
               .filter((person) => person.type === personType)
               .filter((person) => {

                return Object.entries(criteria).every(([key, value ]) => 
                person[key as keyof Person] === value)
          });
        }



## Example Usage


         export const persons: Person[] = [
              { type: 'user', name: 'Max Mustermann', age: 25, occupation: 'Chimney sweep' },
              { type: 'admin', name: 'Jane Doe', age: 32, role: 'Administrator' },
              { type: 'user', name: 'Kate Müller', age: 23, occupation: 'Astronaut' },
              { type: 'admin', name: 'Bruce Willis', age: 64, role: 'World saver' },
              { type: 'user', name: 'Wilson', age: 23, occupation: 'Ball' },
              { type: 'admin', name: 'Agent Smith', age: 23, role: 'Anti-virus engineer' }
           ];


           export const usersOfAge23 = filterPersons(persons, 'user', { age: 23 });
           export const adminsOfAge23 = filterPersons(persons, 'admin', { age: 23 });


           console.log('Users of age 23:');
           usersOfAge23.forEach(logPerson);


           console.log();


           console.log('Admins of age 23:');
           adminsOfAge23.forEach(logPerson);
