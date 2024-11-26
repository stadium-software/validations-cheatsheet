# Validation Expressions Cheat Sheet <!-- omit in toc -->

The release of Stadium 6.12 brings some changes to how field validations work in Stadium. Instead of a simple selection from a predefined set of limited options, we can now use expressions to flexibly validate field values. This readme contains expressions to replace all validations currently built into the field validations checkbox list in Stadium. 

## Contents <!-- omit in toc -->
- [Stadium Version](#stadium-version)
- [Required / Not Required](#required--not-required)
  - [Required Strings (TextBoxes, DatePickers, DropDowns \& RadioButtonLists)](#required-strings-textboxes-datepickers-dropdowns--radiobuttonlists)
  - [Required List Selections (CheckBoxLists)](#required-list-selections-checkboxlists)
  - [Required fields indicator \*](#required-fields-indicator-)
  - [Not required](#not-required)
- [Regular Expressions](#regular-expressions)
  - [IsEmail  (TextBoxes)](#isemail--textboxes)
  - [IsAmount  (TextBoxes)](#isamount--textboxes)
  - [IsNumber  (TextBoxes)](#isnumber--textboxes)
  - [IsURL (TextBoxes)](#isurl-textboxes)
- [Extended Examples](#extended-examples)
  - [Date Range (DatePicker)](#date-range-datepicker)
  - [OnlyCharacters (TextBoxes)](#onlycharacters-textboxes)
  - [TextLength at least 8 (TextBoxes)](#textlength-at-least-8-textboxes)
  - [IsPassword (TextBoxes)](#ispassword-textboxes)

## Stadium Version
Stadium 6.12+

## Required / Not Required
By default the "IsValid Rule" property does not validate any control property. 

### Required Strings (TextBoxes, DatePickers, DropDowns & RadioButtonLists)
When a string property is required, an "IsValid Rule" can simply reference the property to be validated. JavaScript will return *false* when there is no value or when the result is null and undefined. 

**Format**
```javascript
ControlName.PropertyName
```

**Required Strings Examples**
```javascript
TextBox.Text
DatePicker.Date
DropDown.SelectedOption.value
RadioButtonList.SelectedOption.value
```

![](images/required-validation-textbox.png)

### Required List Selections (CheckBoxLists)
When a selection from a List is required, we can check the length of the selected options list.

**Required List Example**
```javascript
CheckBoxList.SelectedOptions.length > 0
```

### Required fields indicator *
To append a * to a form field, add the class "required-indicator" to the classes list of the control

**Properties Panel Class**

![](images/required-indicator-properties-panel.png)

**Result**

![](images/required-inicator-view.png)

### Not required
If a field is not required, but optionally provided values must conform to a specific format, then our "IsValid Rule" should return true in two instances, if:
1. The property value is empty, null or undefined
   OR ( || )
2. If the property conforms to the required format

**Example Expression: returns true if the value is empty, null or undefined OR the value length exceeds 8 characters**
```javascript
!TextBox.Text || TextBox.Text.length > 8
```

## Regular Expressions
A wide range of string validations can be performed using regular expressions

### IsEmail  (TextBoxes)
**Required**
```javascript
TextBox.Text && /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(TextBox.Text)
```

**Not required**
```javascript
!TextBox.Text || /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(TextBox.Text)
```

### IsAmount  (TextBoxes)
**Required**
```javascript
TextBox.Text && /^\d+(\.\d{1,2})?$/.test(TextBox.Text)
```

**Not required**
```javascript
!TextBox.Text || /^\d+(\.\d{1,2})?$/.test(TextBox.Text)
```
### IsNumber  (TextBoxes)
**Required**
```javascript
TextBox.Text && /^\d+$/.test(TextBox.Text)
```

**Not required**
```javascript
!TextBox.Text || /^\d+$/.test(TextBox.Text)
```

### IsURL (TextBoxes)
**Required & with http / https**
```javascript
TextBox.Text && /https?:\/\/[-a-z0-9@:%._\+~#=]{1,256}\.[a-z0-9()]{1,6}\b([-a-z0-9()@:%_\+.~#?&//=]*)/i.test(TextBox.Text)
```

**Required & without http / https**
```javascript
TextBox.Text && /[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)/i.test(TextBox.Text)
```

## Extended Examples

### Date Range (DatePicker)
**Required & date between Jan 1, 2023 & today**
```javascript
DatePicker.Date && DatePicker.Date > new Date('01/01/2023') && DatePicker.Date < new Date()
```

**Not required & date between Jan 1, 2023 & today**
```javascript
!DatePicker.Date || DatePicker.Date > new Date('01/01/2023') && DatePicker.Date < new Date()
```

### OnlyCharacters (TextBoxes)
**Required**
```javascript
TextBox.Text && /^[a-zA-Z]*$/.test(TextBox.Text)
```

**Not required**
```javascript
!TextBox.Text || /^[a-zA-Z]*$/.test(TextBox.Text)
```

### TextLength at least 8 (TextBoxes)
**Required**
```javascript
TextBox.Text && TextBox.Text.length > 7
```

**Not required**
```javascript
!TextBox.Text || TextBox.Text.length > 7
```

### IsPassword (TextBoxes)
**Rules: 8 – 16 characters, at least one number, at least one special character**
```javascript
TextBox.Text && /^(?=.*[\d])(?=.*[!@#$%^&*])[\w!@#$%^&*]{8,16}$/.test(TextBox.Text)
```