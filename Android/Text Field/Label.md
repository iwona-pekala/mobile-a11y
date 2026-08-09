# Text fields and TalkBack

Material 3 [`TextField`](https://developer.android.com/reference/kotlin/androidx/compose/material3/TextField.composable) should be the first choice if possible.

Its `label` is not only visual. It is included in the field's accessibility semantics and effectively serves as the field's accessible name for TalkBack. Other content provided through the component's slots, such as supporting text or icons, also contribute to the resulting accessibility information.

`TextField` provides [`labelPosition`](https://developer.android.com/reference/kotlin/androidx/compose/material3/TextFieldLabelPosition), but the available layouts are still limited. When the required design cannot be created with the Material component, use [`BasicTextField`](https://developer.android.com/reference/kotlin/androidx/compose/foundation/text/BasicTextField.composable).

## Do not build text field semantics with `contentDescription`

Do not use `contentDescription` to recreate a text field's label, value, supporting text, error message, or other visible content.

A `contentDescription` applied directly to the text field can interfere with TalkBack's native handling of editable text, including navigation and editing.

Applying it to an outer container is not an option either, as TalkBack ignores it.

Similarly, avoid manually combining the field and its surrounding content with `mergeDescendants`. This option won't work either.

**Keep the editable field's native semantics intact.**

## Put related content in the decorator

`BasicTextField` allows custom content to be placed around `innerTextField` using its `decorator`.

```kotlin
val state = rememberTextFieldState()

BasicTextField(
    state = state,
    decorator = { innerTextField ->
        Column {
            Row {
                Text("Email address")
                innerTextField()
            }
            Text("We’ll use this address to contact you")
        }
    }
)
```

The decorator can contain any required layout: labels, borders, prefixes, suffixes, supporting text, error messages, icons, or other UI.

**Keep these elements as real UI content and preserve the text field's native semantics. Do not try to reproduce them with `contentDescription` anywhere in the text field hierarchy.**

**Note:** This guidance does not prevent leading or trailing icons from having their own accessibility information when applicable. Adding an accessible name to an icon is not the same as replacing the semantics of a complex control such as a text field.
