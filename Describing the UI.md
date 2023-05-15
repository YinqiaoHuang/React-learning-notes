# 🎨 Describing the UI
## What's JSX?
## What's a "component"?
React components are **regular JavaScript functions** except:

- Their names always **begin with a capital letter**.
- They return JSX markup.

## 3 steps to build a component
### Step 1: Export the component
Add `export default` prefix.
### Step 2: Define the function
Exactly like writing a JavaScript function.

Components' names must start with a capital letter or they won’t work!

### Step 3: Add markup
If your markup isn’t all on the same line as the `return` keyword, you must wrap it in a pair of parentheses or any code on the lines after `return` will be ignored!