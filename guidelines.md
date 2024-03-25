# FE Guidelines

## Table of Contents

### :white_check_mark: NEVER use "else" and avoid using "switch"
Early return or use object mapping

:no_entry_sign: NOT THIS
```JavaScript
function GenderBadge(gender: TGender) {
  if (gender === "M") return <Badge>Male</Badge>
  else if (gender === "F") return <Badge>Female</Badge>
  else return <Badge>N/A</Badge>
}
// OR THIS
function GenderBadge(gender: TGender) {
  switch (gender) {
    case "M": 
      return <Badge>Male</Badge>
    case "F":
      return <Badge>Female</Badge>
    default:
      return <Badge>N/A</Badge>
  }
}
```
:white_check_mark: DO THIS
```JavaScript
function GenderBadge(gender: TGender) {
  if (gender === "M") return <Badge variant="success">Male</Badge>
  if (gender === "F") return <Badge variant="danger">Female</Badge>
  return <Badge>N/A</Badge>
}
```
:hearts: or THIS
```JavaScript
const MAP_BADGE = {
  M: { label: 'Male', variant: 'green', },
  F: { label: 'Female', variant: 'red', },
  default: { label: 'N/A', variant: '', },
}

function GenderBadge(gender: TGender) {
  return <Badge variant={MAP_BADGE[gender].variant}>{MAP_BADGE[gender]?.label ?? MAP_BADGE.default.label}</Badge>;
}
```
Note: Do not use MAP react components
```
{ M: <Badge>Male<Badge> } // BAD
```


### :white_check_mark: Utilize index for component imports with import/resolver
:no_entry_sign: NOT THIS
```Javascript
import Form from '../../ui/components/form/Form'
import Input from '../../ui/components/input/Input'
```
:white_check_mark: DO THIS
```Javascript
import { Form, Input } from 'ui/components'; // THIS

import { Form, Input } from '@ui/components'; // OR THIS
```

### :white_check_mark: When to use the right Case
- kebab for folders: `my-folder`
- pascal for components: `MyComponent`
- camel for props: `onClick`
- camel for hooks: `useMyHook`
- camel for functions: `triggerMyFunction`
- upper snake for constants: `CONSTANTS`
- camel for function parameters: `const foo = (myParameter) =>`
- snake for asset files: `my_image.png`
- camel for internal variables: `myVariable`
- snake for variables(from API): `my_variable`

### :white_check_mark: Variable Naming

```Javascript
// for formatted values, add *_human at the end
{
  created_at: '2022-01-01 01:00:00',
  created_at_human: 'Jan 01, 2022 1:00am',
  amount: 1000,      
  amount_human: '1,000.00',
}
```

### :white_check_mark: Transformer Best Practice
```Javascript
// instead of calling formatter in component level
// Header.js
<div>{profile.first_name} {profile.last_name}</div>
<div>{formatDate(profile.birth_date)}</div>
// List.js
<div>{profile.first_name} {profile.last_name}</div>

// utilize transformer
{
  full_name: `${first_name} ${last_name}`,
  birth_date_human: formatDate(profile.birth_date),
  ...
}
// Header.js
<div>{profile.full_name}</div>
<div>{profile.birth_date_human)}</div>
// List.js
<div>{profile.full_name}</div>
```

### :white_check_mark: Where to put asset files (styles, images)
```Javascript
// instead of importing assets directly from the file
import placeholder from 'assets/images/placeholder.png';
import bannerMain from 'assets/images/banner/main.png';

// create an index file
import { placeholder, bannerMain } from 'assets/images';
```

### :white_check_mark: Function Names

`{verb}{noun}`
```Javascript
// BAD
// handleOpen - not descriptive enough
// handleOnOpenModal - redundant

// GOOD
// openUserModal - descriptive
<button onClick={editUser} type="button">Edit</button>
```

### Constant Names
```Javascript
// if constant is exported, use more descriptive label
export const INIT_USER_FORM    // for form
export const INIT_USER_FILTER  // for filter

// if user ONLY internally within module
export const INIT_FORM    // for form
export const INIT_FILTER  // for filter
```

### Be consistent with Naming related variables
```Javascript
// BAD
const useGetUser = () => {

}
const transformPerson () => ({})
const USERS = 'CONSTANT_USER',

// GOOD
const useUserList = () => {

}
const transformUser = () => ({})
const USER_LIST = 'USER_LIST'
```

### HOOK STANDARD
`// TODO: FOR FURTHER DISCUSSION START`
```Javascript
// PIPZ - CUGGR
useCreateUser
useUpdateUser
useGetUserList
useGetUser
useRemoveUsers

// alee - CULBD
useCreateUser
useUpdateUser
{ list, pager } useUserList
{} useUserById
useDeleteUser

// jerico 
[] = useUserList
{ list, pager } useUserListData
useUserDataQuery({ id: '1', queryOptions: {} })
use[*]Mutation
```
`// TODO: FOR FURTHER DISCUSSION END`

### Toast Standard format
- Collab with TEAM per project

### Recommnded Plugins/Extensions
- ES7+ React/Redux/React-Native snippets
- GitLens — Git supercharged
- Hex to RGB
- Tailwind CSS IntelliSense
- Multiple cursor case preserve

### Editor Rules
- Indent with spaces (2)
- Enable Vertical Ruler [80, 120]

### Naming Rules
- use clear and descriptive naming
```Javascript
// BAD
const [a, b] = usehook();
function user() => { /* ... */}

// GOOD
const [isLoading, list] = useHook();
function getUserList(){ /* ... */}
```

- skip redundancy
```json
// BAD
{
  "user": {
    "user_name": "Juan",
    "user_age": 20
  }
}
// GOOD
{
  "user": {
    "name": "Juan",
    "age": 20
  }
}
```

- make sure that the filename matches the default export name
```
  // for the file UserList.js
  export default UserList;
```

### Code Splitting Guidelines
- implmenent code splitting
  when NOT to do: // TODO: FOR FURTHER DISCUSSION
  when to do:     // TODO: FOR FURTHER DISCUSSION

### Github Branch Naming
- `main`: used for production 
- `staging`: used for uat/staging
- `test`: used for qa testing
- `develop`: used for development (typically set as the default branch)

- When making a feature, branchout with the prefix `feat/*`. e.g `feat/user-management`
- When fixing a bug, branchout with the prefix `bug/*`. e.g `bug/fixed-typo`

### Github Commit Rules
`// TODO: define rules here`

### General Rules
- Do not leave console.logs without purpose; you may use console.error, console.warn
- Resolve errors/warnings on the console (no key on loops, resolve depreciated, etc.)
- There must be no warnings committed to the main branch: `develop`, `test`, `staging` or `main`

### Security
- DO NOT push env values to the repo
- DO NOT push hardcoded credentials
- when using `dangerouslySetInnerHtml`, use package such us [DOM Purify](https://www.npmjs.com/package/dompurify) to sanitize and prevent injection.
