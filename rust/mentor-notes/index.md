# Mentor notes

* How is the project structure so far?

* Is there a console similar to Rails console, where debugging can happen on the fly?

* Review `axum/hello_world/src/routes/custom_json_extractor.rs`

  * How do you feel about the way the `CustomJsonExtractor` is implemented?
  * How do we make it so the response of the StatusCode::BAD_REQUEST and StatusCode::UNPROCESSABLE_ENTITY
    are returned as JSON?
  * Should validation be handled in a custom extractor? Or better to directly in the handler?

* When do use `"string".to_string()` vs `String::from("string")` vs `"string".to_owned()`?

* At `/Users/foopub/Projects/rust/axum/data/src/routes/update_tasks.rs` and `/Users/foopub/Projects/rust/axum/data/src/routes/create_tasks.rs`:

  * How do you return a JSON object response instead of a blank response?
  * Are the `.await.map_err(..)` correct? Should we instead also be checking for `StatusCode::NOT_FOUND` instead of just `INTERNAL_SERVER_ERROR`?

At `https://www.youtube.com/watch?v=9_jBBAsFqwY&list=PLrmY5pVcnuE-_CP7XZ_44HN-mDrLQV4nS&index=34` (/rust/axum/data/src/routes/partial_update_tasks.rs)

  * In comments, some says, you can use a DTO (Data Transfer Object) to represent the data that is being sent to the server. How?

Following https://www.youtube.com/watch?v=NVTO9ebBask&list=PLrmY5pVcnuE-_CP7XZ_44HN-mDrLQV4nS&index=43
for setting up JWT authentication. Some concerns:

* The crate metnioned (https://github.com/Keats/jsonwebtoken) hasnt been updated since a few months
* Is it recommended to use a 3rd party service like Auth0 instead?
* Or should use another crate?


