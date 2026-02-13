Temporal از Event Sourcing استفاده می‌کند.

به جای ذخیره State نهایی، کل رویدادها ذخیره می‌شوند:

- WorkflowExecutionStarted
    
- WorkflowTaskScheduled
    
- WorkflowTaskCompleted
    
- WorkflowExecutionCompleted
    

اگر Worker Crash کند:

Temporal کل History را Replay می‌کند  
و State را بازسازی می‌کند.

این یعنی:

- Durability واقعی
    
- Fault Tolerance
    
- Crash Recovery بدون از دست دادن State