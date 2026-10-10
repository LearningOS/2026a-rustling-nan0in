diff --git a/exercises/conversions/from_into.rs b/exercises/conversions/from_into.rs
index aba471d..81d05dc 100644
--- a/exercises/conversions/from_into.rs
+++ b/exercises/conversions/from_into.rs
@@ -7,6 +7,8 @@
 // Execute `rustlings hint from_into` or use the `hint` watch subcommand for a
 // hint.
 
+use std::default;
+
 #[derive(Debug)]
 struct Person {
     name: String,
@@ -44,6 +46,31 @@ impl Default for Person {
 
 impl From<&str> for Person {
     fn from(s: &str) -> Person {
+        if s.is_empty() {
+            return Person::default();
+        }
+
+        let parts:Vec<&str> = s.split(',').collect();
+
+        if parts.len() != 2 {
+            return Person::default();
+        }
+        
+        let name = parts[0];
+        if name.is_empty() {
+            return Person::default();
+        }
+
+        let age = match parts[1].parse::<usize>(){
+            Ok(age) => age,
+            Err(_) => return Person::default(),
+        };
+
+        Person{
+            name: name.to_string(),
+            age,
+        }
+
     }
 }
 
