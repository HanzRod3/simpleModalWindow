### Learning to Manipulate the DOM with Classes

We have a `<div>` with the classes `modal` and `hidden`, and we are going to manipulate this element so it appears as a modal window over the webpage. The modal window will overlay the page, and a close button inside the modal allows the user to close it. Here's the HTML structure:

```html
<div class="modal hidden">
  <button class="close-modal">&times;</button>
  <h1>I'm a modal window 😍</h1>
  <p>Modal window content goes here.</p>
</div>
<div class="overlay hidden"></div>
```

- **Modal**: The `hidden` class initially hides this modal window.
- **Close Button**: `<button class="close-modal">&times;</button>` lets the user close the modal.
- **Overlay**: The overlay covers the background when the modal is open. It also has a `hidden` class by default.

### Querying DOM Elements

We use `querySelector` to select elements in the DOM. If you have multiple elements with the same class name, like `.show-modal`, `querySelector` only selects the **first** one. To select all elements with the same class, you use `querySelectorAll`, which returns a NodeList (a collection of elements).

```javascript
const btnsOpenModal = document.querySelectorAll('.show-modal');
```

This selects all buttons that have the class `show-modal`, allowing each button to trigger the modal to open.

### Removing and Adding Classes

When the modal is hidden, the `hidden` class keeps it invisible. We can make the modal visible by removing this class using `modal.classList.remove('hidden')`.

```javascript
modal.classList.remove('hidden'); // Makes the modal appear
overlay.classList.remove('hidden'); // Shows the overlay
```

At this point, we only have the functionality to show the modal, but we haven't yet implemented the ability to close it. To close the modal, we need to add back the `hidden` class:

```javascript
modal.classList.add('hidden');
overlay.classList.add('hidden');
```

### Useful Use Cases for `classList`

- **Toggling visibility**: You can add or remove the `hidden` class based on user actions, making elements appear or disappear.
- **Changing themes**: You can toggle between light and dark modes by adding/removing a `dark-mode` class.
- **Interactive elements**: Use `classList` to apply different styles dynamically, such as highlighting a selected element or expanding a card when clicked.

### Keeping the Code DRY (Don’t Repeat Yourself)

To make our code more efficient, we created a function `closeModal` that holds the logic to close the modal and the overlay. Both the close button and the overlay use the same logic, so we pass `closeModal` as the callback function to the event listeners:

```javascript
const closeModal = () => {
  modal.classList.add('hidden');
  overlay.classList.add('hidden');
};

btnCloseModal.addEventListener('click', closeModal);
overlay.addEventListener('click', closeModal);
```

#### Why Don’t We Add `()` to `closeModal` in Event Listeners?

If you add `()` when passing a function to an event listener, it will execute immediately when the page loads, instead of when the event occurs. In this case, you want the function to run **only** when the user clicks the button or overlay, so you pass the function reference without `()`:

```javascript
btnCloseModal.addEventListener('click', closeModal); // Correct
```

### Adding a Keyboard Event Listener

We can also close the modal when the user presses the **Escape** key. To detect keypresses, we use a global `keydown` event listener:

```javascript
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') {
    if (!modal.classList.contains('hidden')) {
      closeModal();
    }
  }
});
```

#### Why Do We Need to Add `()` to `closeModal()` Here?

In this case, `closeModal()` needs to be executed **inside** the `if` statement when the condition is met. Therefore, we include the `()` to call the function when the keypress condition (Escape key and modal being visible) is satisfied.

### Refactoring the `if` Statement

We can simplify the nested `if` statement into a single condition:

```javascript
document.addEventListener('keydown', e => {
  if (e.key === 'Escape' && !modal.classList.contains('hidden')) {
    closeModal();
  }
});
```

### Summary of Key Concepts:

1. **Class Manipulation**:

   - **`.classList.add()`**: Adds a class to an element.
   - **`.classList.remove()`**: Removes a class from an element.
   - **`.classList.toggle()`**: Adds the class if it’s missing or removes it if it’s present.

2. **Event Listeners**:

   - Use `addEventListener` to listen for events like clicks or keypresses. In this example, we use event listeners for opening and closing the modal.

3. **Keyboard Events**:

   - Listen for specific keypresses with the `keydown` event, and use conditions to handle actions based on the key pressed.

4. **Keeping Code DRY**:
   - Reuse functions like `closeModal` to avoid repeating code. This makes your code cleaner and easier to maintain.

---
