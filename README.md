# Multi Step Dynamic Form in Angular
Passing formname, form fields, step details to create dynamic multi step form with prev/next button
1. Save button should be clicked to save data
2. Click on prev button to go previous form with already submitted value
3. At last step, you will get submit button instead of next to save total data

### Run the application
```
ng serve
localhost:4200
```

![Multi Step Dynamic Form in Angular](multistepform.png)

### App Component
static JSON file

```
this.data = [
      {
        'stepname': 'basic',
        'formFields': [
          {
          'key': 'firstName',
          'input': 'text',
          'valids': [{
              'valid': 'required',
              'error': 'firstName is required'
            },
            {
              'valid': 'pattern',
              'validator': '^[a-zA-Z]+$',
              'error': 'firstName is accept only text'
            },
            {
              'valid': 'minlength',
              'length': 3,
              'error': 'firstName must be at least 3 characters'
            }
          ]
          },
          {
            'key': 'middleName',
            'input': 'text',
            'valids': []
          },
          {
            'key': 'lastName',
            'input': 'text',
            'valids': [{
                'valid': 'required',
                'error': 'lastName is required'
              },
              {
                'valid': 'pattern',
                'validator': '^[a-zA-Z]+$',
                'error': 'lastName is accept only text'
              },
              {
                'valid': 'minlength',
                'length': 3,
                'error': 'lastName must be at least 3 characters'
              }
            ]
          },
          {
            'key': 'marital status',
            'input': 'select',
            'items': [{
                'name': 'married',
                'id': 0
              },
              {
                'name': 'unmarried',
                'id': 1
              }
            ],
            'valids': []
          },
          {
            'key': 'gender',
            'input': 'radio',
            'items': [{
                'name': 'male',
                'id': 0
              },
              {
                'name': 'female',
                'id': 1
              }
            ],
            'valids': []
          }
        ]
      },
      {
        'stepname': 'contact',
        'formFields': [
          {
            'key': 'emailId',
            'input': 'email',
            'valids': [{
                'valid': 'required',
                'error': 'emailId is required'
              },
              {
                'valid': 'emailId',
                'error': 'emailId must be valid'
              }
            ]
          },
          {
            'key': 'mobile',
            'input': 'text',
            'valids': [{
                'valid': 'required',
                'error': 'mobile is required'
              },
              {
                'valid': 'pattern',
                'validator': '^[0-9]{10}$',
                'error': 'mobile is accept only number and maximum 10 numbers '
              }
            ]
          },
        ]
      },
      {
        'stepname': 'other',
        'formFields': [
          {
            'key': 'country',
            'input': 'text',
            'valids': [{
                'valid': 'required',
                'error': 'country is required'
              },
            ]
          },
          {
            'key': 'state',
            'input': 'text',
            'valids': [{
                'valid': 'required',
                'error': 'state is required'
              },
            ]
          }
        ]
      }
    ];
```

### Multistepform component
Passing all details though the form component

```
<app-form *ngIf="stepItems && startingIndex < countSteps"
    [countSteps]="countSteps"
    [stepNo]="startingIndex"
    [formFields]="data[startingIndex].formFields"
    [formValues]="formValues"
    [stepName]="data[startingIndex].stepname"
    (formData)="getFormData($event)"
    (newStep)="onnewStep($event)"
    ></app-form>
```

### Form component
Dynamically form is generating regarding passsing values from multistepform component

```
<form [formGroup]="formName" #formDir="ngForm">
<div class="row" *ngFor="let form of formFields; let i = index">
    <div class="col-12 form-group" *ngIf="form.input == 'text'">
      <label>{{form.key}}
        <span class="required" *ngIf="formName.get(form.key).hasError('required')">*</span>
      </label>
      <input [type]="form.input" [formControlName]="form.key" class="form-control" [ngClass]="{'is-invalid' : formName.get(form.key).errors && formDir.submitted }">
      <div *ngIf="!formName.get(form.key).valid && formName.get(form.key).errors && formDir.submitted">
        <div *ngFor="let err of form.valids; let k = index">
          <span class="error" *ngIf="formName.get(form.key).hasError(err.valid)">{{err.error}}</span>
        </div>
      </div>
    </div>
    <div class="col-12 form-group" *ngIf="form.input == 'email'">
      <label>{{form.key}}
        <span class="required" *ngIf="formName.get(form.key).hasError('required')">*</span>
      </label>
      <input [type]="form.input" [formControlName]="form.key" class="form-control" [ngClass]="{'is-invalid' : formName.get(form.key).errors && formDir.submitted }">
      <div *ngIf="!formName.get(form.key).valid && formName.get(form.key).errors && formDir.submitted">
        <div *ngFor="let err of form.valids; let k = index">
          <span class="error" *ngIf="formName.get(form.key).hasError(err.valid)">{{err.error}}</span>
        </div>
      </div>
    </div>
    <div class="col-12 form-group" *ngIf="form.input == 'password'">
      <label>{{form.key}}
        <span class="required" *ngIf="formName.get(form.key).hasError('required')">*</span>
      </label>
      <input [type]="form.input" minlength="3" [formControlName]="form.key" class="form-control" [ngClass]="{'is-invalid' : formName.get(form.key).errors && formDir.submitted }">
      <div *ngIf="!formName.get(form.key).valid && formName.get(form.key).errors && formDir.submitted">
        <div *ngFor="let err of form.valids; let k = index">
          <span class="error" *ngIf="formName.get(form.key).hasError(err.valid)">{{err.error}}</span>
        </div>
      </div>
    </div>
    <div class="col-12 form-group" *ngIf="form.input == 'select'">
      <label>{{form.key}}
        <span class="required" *ngIf="formName.get(form.key).hasError('required')">*</span>
      </label>
      <select [formControlName]="form.key" class="form-control" [ngClass]="{'is-invalid' : formName.get(form.key).errors && formDir.submitted }">
        <option *ngFor="let item of form.items; let j = index" [value]="item.id" [selected]="formValues[form.key]">
          {{item.name}}
        </option>
      </select>
      <div *ngIf="!formName.get(form.key).valid && formName.get(form.key).errors && formDir.submitted ">
        <div *ngFor="let err of form.valids; let k = index">
          <span class="error" *ngIf="formName.get(form.key).hasError(err.valid)">{{err.error}}</span>
        </div>
      </div>
    </div>
    <div class="col-12 form-group" *ngIf="form.input == 'radio'">
      <label>{{form.key}}
        <span class="required" *ngIf="formName.get(form.key).hasError('required')">*</span>
      </label>
      <div class="form-group">
        <div class="form-check-inline" *ngFor="let item of form.items; let j = index">
          <label class="form-check-label">
            <input [type]="form.input" class="form-check-input" [formControlName]="form.key" [value]="item.id"
              [ngClass]="{'is-invalid' : formName.get(form.key).errors && formDir.submitted }" name="form.key" [checked]="formValues[form.key]">{{item.name}}
          </label>
        </div>
        <div *ngIf="!formName.get(form.key).valid && formName.get(form.key).errors && formDir.submitted">
          <div *ngFor="let err of form.valids; let k = index">
            <span class="error" *ngIf="formName.get(form.key).hasError(err.valid)">{{err.error}}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="col-12 form-group" *ngIf="form.input == 'checkbox'">
      <label>{{form.key}}
        <span class="required" *ngIf="formName.get(form.key).hasError('required')">*</span>
      </label>
      <div class="form-group">
        <div class="form-check-inline" *ngFor="let item of form.items; let j = index">
          <label class="form-check-label">
            <input [type]="form.input" class="form-check-input" [formControlName]="form.key" [value]="item.id"
              [ngClass]="{'is-invalid' : formName.get(form.key).errors && formDir.submitted }" name="form.key" [checked]="formValues[form.key]">{{item.name}}
          </label>
        </div>
        <div *ngIf="!formName.get(form.key).valid && formName.get(form.key).errors && formDir.submitted">
          <div *ngFor="let err of form.valids; let k = index">
            <span class="error" *ngIf="formName.get(form.key).hasError(err.valid)">{{err.error}}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="col-6 form-group text-left">
      <button type="button" class="btn btn-default" [disabled]="stepNo === 0" (click)="gotoStep(stepNo-1)">Prev</button>
    </div>
    <div class="col-6 form-group text-right">
      <button type="submit" *ngIf="stepNo !== countSteps - 1" class="btn btn-success" (click)="submitForm()">Next</button>
      <button type="submit" *ngIf="stepNo === countSteps - 1" class="btn btn-success" (click)="submitForm()">Submit</button>
    </div>
  </div>
  </form>

```



