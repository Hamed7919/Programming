Our definition is that a workflow is a:

Resilient Program, that
Executes Tasks, and
Reacts to External Events, including
Timers and timeouts
___
- If your program restarts or the backend service goes down, your program will be in exactly the same state with all local variables and stack traces in exactly the same state.
- You can call sleep for 30 days in your code without caring about process restarts or dealing with databases and state recovery, because that is done automatically by Temporal.
___
