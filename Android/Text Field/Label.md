# Text fields and TalkBack

Material 3 [`TextField`](https://developer.android.com/reference/kotlin/androidx/compose/material3/TextField.composable) should be the first choice if possible.

Its `label` is not only visual. It is included in the field's accessibility semantics and effectively serves as the field's accessible name for TalkBack. Other content provided through the component's slots, such as supporting text or icons, also contribute to the resulting accessibility information.

`TextField` provides [`labelPosition`](https://developer.android.com/reference/kotlin/androidx/compose/material3/TextFieldLabelPosition), but the available layouts are still limited. When the required design cannot be created with the Material component, use [`BasicTextField`](https://developer.android.com/reference/kotlin/androidx/compose/foundation/text/BasicTextField.composable).

## Do not build text field semantics with `contentDescription`

Do not use `contentDescription` to recreate a text field's label, value, supporting text, error message, or other visible content.

A `contentDescription` applied directly to the text field can interfere with TalkBack's native handling of editable text, including navigation and editing.

Applying it to an outer container is not a reliable alternative: TalkBack may ignore it or expose the container separately instead of using it to label the text field.

Similarly, avoid manually combining the field and its surrounding content with `mergeDescendants`. This option won't work either.

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
Do not try to include them in the text field's contentDescription.

**Note:** contentDescription may interfere with text editing operations when applied to the text field, and does not name the field when applied to its parent. It can still be used on child elements, such as icons, when applicable.
