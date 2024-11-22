# Validation Expressions Cheat Sheet <!-- omit in toc -->

The release of Stadium 6.12 brings some changes to how field validations work in Stadium. Instead of a simple selection from a predefined set of limited options, we can now use expressions to flexibly validate field values. 

The new feature also comes with an option to write a specific error text with each field. Validations can also be triggered from inside of scripts and event handlers and the error text can be re-defined as needed. This means we can craft error messages that help users understand what values are needed to get rid of the error. 

## Contents <!-- omit in toc -->
- [Required fields indicator \*](#required-fields-indicator-)
- [Required Strings (TextBoxes, DatePickers, DropDowns \& RadioButtonLists)](#required-strings-textboxes-datepickers-dropdowns--radiobuttonlists)
- [Required List Selections (CheckBoxLists)](#required-list-selections-checkboxlists)
- [IsEmail  (TextBoxes)](#isemail--textboxes)
- [IsAmount  (TextBoxes)](#isamount--textboxes)
- [IsNumber  (TextBoxes)](#isnumber--textboxes)
- [Date Range (DatePicker)](#date-range-datepicker)
- [IsURL (TextBoxes)](#isurl-textboxes)
- [OnlyCharacters (TextBoxes)](#onlycharacters-textboxes)
- [TextLength at least 8 (TextBoxes)](#textlength-at-least-8-textboxes)
- [IsPassword (TextBoxes)](#ispassword-textboxes)

## Required fields indicator *
To append a * to a form field, add the class "required-indicator" to the classes list of the control

## Required Strings (TextBoxes, DatePickers, DropDowns & RadioButtonLists)
```javascript
TextBox.Text
DatePicker.Date
DropDown.SelectedOption.value
RadioButtonList.SelectedOption.value
```

## Required List Selections (CheckBoxLists)
```javascript
CheckBoxList.SelectedOptions.length > 0
```

## IsEmail  (TextBoxes)
**Required**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/)
```

**Not required**
```javascript
!TextBox.Text || TextBox.Text.toLowerCase().match(/^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|.(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/)
```

## IsAmount  (TextBoxes)
**Required**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/^\d+(\.\d{1,2})?$/)
```

**Not required**
```javascript
!TextBox.Text || TextBox.Text.toLowerCase().match(/^\d+(\.\d{1,2})?$/)
```
## IsNumber  (TextBoxes)
**Required**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/^\d+$/)
```

**Not required**
```javascript
!TextBox.Text || TextBox.Text.toLowerCase().match(/^\d+$/)
```

## Date Range (DatePicker)
**Required & date between Jan 1, 2023 & today**
```javascript
DatePicker.Date && DatePicker.Date > new Date('01/01/2023') && DatePicker.Date < new Date()
```

## IsURL (TextBoxes)
**Required & with http / https**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)/)
```

**Required & without http / https**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)/)
```

## OnlyCharacters (TextBoxes)
**Required**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/^[a-zA-Z]*$/)
```

**Not required**
```javascript
!TextBox.Text || TextBox.Text.toLowerCase().match(/^[a-zA-Z]*$/)
```

## TextLength at least 8 (TextBoxes)
**Required**
```javascript
TextBox.Text && TextBox.Text.length > 7
```

**Not required**
```javascript
!TextBox.Text || TextBox.Text.length > 7
```

## IsPassword (TextBoxes)
**Required & Rules: 8 – 16 characters, at least one number, at least one special character**
```javascript
TextBox.Text && TextBox.Text.toLowerCase().match(/^(?=.*[\d])(?=.*[!@#$%^&*])[\w!@#$%^&*]{8,16}$/)
```