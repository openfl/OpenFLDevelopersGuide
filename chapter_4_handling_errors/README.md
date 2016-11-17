# Chapter 4: Handling errors {#chapter-4-handling-errors}

Flash Player 9 and later, Adobe AIR 1.0 and later

To “handle” an error means that you build logic into your application to respond to, or fix, an error. Errors are generated either when an application is compiled or when a compiled application is running. When your application handles errors, _something_ occurs as a response when the error is encountered, as opposed to no response (when whatever process created the error fails silently). Used correctly, error handling helps shield your application and its users from otherwise unexpected behavior.

However, error handling is a broad category that includes responding to many kinds of errors that are thrown during compilation or while an application is running. This discussion focuses on how to handle run-time errors (thrown while an application is running), the different types of errors that can be generated, and the advantages of the error- handling system in ActionScript 3.0.