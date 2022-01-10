# Display Sleep

[Chritoher Masto's Youtube video](https://www.youtube.com/watch?v=zndPIPLRjb8) has some good tips and insights about this topic. However I think I found out how to utilize the official sleep function.

#### Nextion Sleep

In the [Nextion Editor](https://nextion.tech/nextion-editor/) I added two commands to "Preinitialize Events" of the first page. Check [Nextion Instruction Set](https://nextion.tech/instruction-set/) for more.

    // Enable wake on touch
    thup=1
    // set sleep on no touch
    thsp=15

This will send the display to sleep after 15 seconds.


[Back](README.md)
