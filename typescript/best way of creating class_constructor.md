# Create class for backend and frontend project:

- ***This way, you can pass properties in any order and even omit some properties if they're optional.***

```Ts
interface DepartmentParams {
  deptId?: number;
  deptName?: string;
  deptHeadEmpId?: number;
  createdDate?: Date;
  deptHeadName?: string;
  slNo?: number;
}

export class Department {
  deptId: number;
  deptName: string;
  deptHeadEmpId: number;
  createdDate: Date;
  deptHeadName: string;
  slNo?: number;

  constructor({
    deptId = 0,
    deptName = "",
    deptHeadEmpId = 0,
    createdDate = new Date(),
    deptHeadName = "",
    slNo
  }: DepartmentParams = {}) {
    this.deptId = deptId;
    this.deptName = deptName;
    this.deptHeadEmpId = deptHeadEmpId;
    this.createdDate = createdDate;
    this.deptHeadName = deptHeadName;
    this.slNo = slNo;
  }
}


```

### Create instance of this class:

```ts
import { Department } from './department';

// Create instances of the Department class with different properties in any order

const department1 = new Department({ deptId: 1, deptName: 'Engineering', deptHeadEmpId: 101 });

const department2 = new Department({ deptName: 'Marketing', createdDate: new Date('2023-02-01'), deptHeadEmpId: 102 });

const department3 = new Department({ deptHeadEmpId: 103, deptName: 'Sales' });

const department4 = new Department({
  deptId: 4,
  createdDate: new Date('2023-03-01'),
  deptHeadName: 'Jane Smith',
  deptName: 'HR',
  slNo: 1
});

console.log(department1);
/* Output:
Department {
  deptId: 1,
  deptName: 'Engineering',
  deptHeadEmpId: 101,
  createdDate: <current date>,
  deptHeadName: '',
  slNo: undefined
}
*/

console.log(department2);
/* Output:
Department {
  deptId: 0,
  deptName: 'Marketing',
  deptHeadEmpId: 102,
  createdDate: 2023-02-01T00:00:00.000Z,
  deptHeadName: '',
  slNo: undefined
}
*/

console.log(department3);
/* Output:
Department {
  deptId: 0,
  deptName: 'Sales',
  deptHeadEmpId: 103,
  createdDate: <current date>,
  deptHeadName: '',
  slNo: undefined
}
*/

console.log(department4);
/* Output:
Department {
  deptId: 4,
  deptName: 'HR',
  deptHeadEmpId: 0,
  createdDate: 2023-03-01T00:00:00.000Z,
  deptHeadName: 'Jane Smith',
  slNo: 1
}
*/

```

# Using for loop:

```Ts
interface DepartmentParams {
  deptId?: number;
  deptName?: string;
  deptHeadEmpId?: number;
  createdDate?: Date;
  deptHeadName?: string;
  slNo?: number;
}

export class Department {
  deptId: number = 0;
  deptName: string = "";
  deptHeadEmpId: number = 0;
  createdDate: Date = new Date();
  deptHeadName: string = "";
  slNo?: number;

  constructor(params: DepartmentParams = {}) {
    // Loop through the keys of the params object and assign them to the class properties
    for (const key in params) {
      if (params.hasOwnProperty(key) && params[key] !== undefined) {
        (this as any)[key] = params[key];
      }
    }
  }
}

```

# Using another way:
- must set ts strict rules to false
```TS
export class Department {
  deptId: number = 0;
  deptName: string = '';
  deptHeadEmpId: number = 0;
  createdDate: Date = new Date();
  deptHeadName: string = '';
  slNo: number = 0;

  constructor(obj?: any) {
    if (obj) {
      Object.keys(this).forEach((key) => {
        if (obj.hasOwnProperty(key)) {
          this[key] = obj[key];
        }
      });
    }
  }
}


```

