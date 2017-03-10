## Promises are crazy!

Their main use is to sit and wait for the eventual conclusion of an action, which is vital for asynchronous when we have things that we don't know 

   ### Important points

* Success Track

Promises are 'by default' on the success track, as they return `undefined` if nothing is explicitly returned, and will only hit `.then`'s.

* Error Track

If an error is thrown, it will keep going down the chain (skipping things) until it finds an error handler; the error handler should be a `.catch`.

After the `.catch` is fired and leaves its "brackets", the chain is back on the success track as the error was *cleaned* out.

* `.then`

* Promises returning promises

The newly returned promise can change the track that the code is currently on, so watch out.
