diff --git a/exercises/threads/threads2.rs b/exercises/threads/threads2.rs
index 62dad80..a81874a 100644
--- a/exercises/threads/threads2.rs
+++ b/exercises/threads/threads2.rs
@@ -9,7 +9,7 @@
 
 // I AM NOT DONE
 
-use std::sync::Arc;
+use std::sync::{Arc,Mutex};
 use std::thread;
 use std::time::Duration;
 
@@ -18,14 +18,15 @@ struct JobStatus {
 }
 
 fn main() {
-    let status = Arc::new(JobStatus { jobs_completed: 0 });
+    let status = Arc::new(Mutex::new(JobStatus { jobs_completed: 0 }));
     let mut handles = vec![];
     for _ in 0..10 {
         let status_shared = Arc::clone(&status);
         let handle = thread::spawn(move || {
             thread::sleep(Duration::from_millis(250));
             // TODO: You must take an action before you update a shared value
-            status_shared.jobs_completed += 1;
+            let mut status= status_shared.lock().unwrap();
+            status.jobs_completed += 1;
         });
         handles.push(handle);
     }
@@ -34,6 +35,6 @@ fn main() {
         // TODO: Print the value of the JobStatus.jobs_completed. Did you notice
         // anything interesting in the output? Do you have to 'join' on all the
         // handles?
-        println!("jobs completed {}", ???);
+        println!("jobs completed {}",status.lock().unwrap().jobs_completed);
     }
 }
