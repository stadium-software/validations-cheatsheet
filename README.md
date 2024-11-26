# Validation Expressions Cheat Sheet <!-- omit in toc -->

The release of Stadium 6.12 brings some changes to how field validations work in Stadium. Instead of a simple selection from a predefined set of limited options, we can now use expressions to flexibly validate field values. This readme contains expressions to replace all validations currently built into the field validations checkbox list in Stadium. 

## Contents <!-- omit in toc -->
- [Stadium Version](#stadium-version)
- [Required / Not Required](#required--not-required)
  - [Required TextBox, DatePicker, DropDown \& RadioButtonList (Strings)](#required-textbox-datepicker-dropdown--radiobuttonlist-strings)
  - [Required CheckBoxList (List Selection)](#required-checkboxlist-list-selection)
  - [Required field indicator \*](#required-field-indicator-)
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
Ths readme applies to Stadium versions 6.12+

## Required / Not Required
By default the "IsValid Rule" property does not validate any control property. 

### Required TextBox, DatePicker, DropDown & RadioButtonList (Strings)
When a string property is required, the "IsValid Rule" can simply reference the property to be validated. JavaScript will return *false* when the property contains no value and when the property value is null or undefined.

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

### Required CheckBoxList (List Selection)
When a selection from a List is required, we can check the length of the selected options list.

**Required List Example**
```javascript
CheckBoxList.SelectedOptions.length > 0
```

### Required field indicator *
To append a * to a form field, add the class "required-indicator" to the classes list of the control

**Properties Panel Class**

![](images/required-indicator-properties-panel.png)

**Result**

![](images/required-inicator-view.png)

### Not required
If a field is not required, but optionally provided values must conform to a specific format, then it is necessary to craft an expression that returns true when:
1. The property value is empty, null or undefined
2. The property conforms to the required format

In this case, two expressions must be combined with a double-pipe (OR). The first expression returns true if there is no value, and the second expression returns true if there is a value that conforms to a required format. For example:
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