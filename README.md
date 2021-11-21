# Pomoto

## Usage

### Modes:

### mode="css"

A lighter but less flexible rendering method, great for when you don't need all the bells and whistles. Tooltips or simple popovers are great usecases for 'css' mode.

- Rendering done entirely through CSS, (which means it works even with JS completely disabled).
- If you want to position your element relative to a reference element, they will both be wrapped in a 'relative' container, and positioning is done using `position` and `transform`. This extra wrapper element can sometimes interfere with your page layout ('js' mode doesn't have this problem).
- Not passing a reference element means your element will be positioned relative to the page and does not create a wrapper.
- Events such as `on:open` `on:close` and being able to read/manipulate the element `visible` state still work as expected, but require Javascript in order to do so.
- All triggers still work as expected, but only `hover` and `focus` triggers will actually render using CSS.
- Rendering is done through opacity/visibility toggling rather than `{#if}`, which means Svelte transitions cannot work.
- Portals do not work (use 'js' mode for that).

## Accessibility guide

| Component                                                                                                   | Role           | Rendering method | Is focusable | Enter on trigger to open/close | Open/close on focus | Arrow keys move between items | Focus first element | Focus is trapped inside | Escape closes             | On close focus is restored to previous element |
| ----------------------------------------------------------------------------------------------------------- | -------------- | ---------------- | ------------ | ------------------------------ | ------------------- | ----------------------------- | ------------------- | ----------------------- | ------------------------- | ---------------------------------------------- |
| Tooltip: Can only have text content, without any focusable elements                                         | role="tooltip" | CSS only         | no           | no                             | yes                 | no                            | no                  | no                      | yes (due to losing focus) | n/a                                            |
| Popover: Rich content, but without any focusable elements                                                   | role="tooltip" | JS               | no           | yes                            | no                  | no                            | no                  | no                      | yes                       | n/a                                            |
| Popover: Rich content which is less important, modeless, and contains focusable elements                    | role="dialog"  | JS               | yes          | yes                            | no                  | no                            | no                  | no                      | yes                       | yes                                            |
| Modal: Rich content which requires an answer or presents important information, contains focusable elements | role="dialog"  | JS               | yes          | yes                            | no                  | no                            | yes                 | yes                     | yes                       | yes                                            |
| Menu: Interactive content with focusable elements resembling a classic dropdown menu                        | role="menu"    | JS               | yes          | yes                            | no                  | yes                           | no                  | no                      | yes                       | yes                                            |
