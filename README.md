# Error Info Derive Macro

A Rust library for defining structured and user-friendly error messages using custom derive macros. This library simplifies the creation of error types and ensures consistency in error handling across your application.

## Features

- Define structured error types with detailed metadata.
- Generate client-friendly and server-detailed error messages.
- Automatic hash generation for error messages to uniquely identify issues.
- Provides a custom derive macro `ToErrorInfo` for easily converting enums to structured errors.

## Usage

### Example

Define an error enum using the `ToErrorInfo` derive macro:

```rust
use thiserror::Error;
use error_info_derive::ToErrorInfo;

#[derive(Error, ToErrorInfo)]
#[error_info(app_type = "StatusCode", prefix = "01")]
pub enum MyError {
    #[error("Invalid command: {0}")]
    #[error_info(code = "IC", app_code = "400")]
    InvalidCommand(String),

    #[error("Invalid argument: {0}")]
    #[error_info(code = "IA", app_code = "400", client_msg = "Friendly message")]
    InvalidArgument(String),

    #[error("{0}")]
    #[error_info(code = "RE", app_code = "500")]
    RespError(#[from] RespError),
}
```

This macro will generate implementations for the `ToErrorInfo` trait and handle conversion of your error enum into structured `ErrorInfo` instances.


## How It Works

1. The `ToErrorInfo` derive macro processes enums annotated with `#[error_info]`.
2. It generates implementations to convert error variants into `ErrorInfo` instances containing:
   - `app_code`: Application-specific error codes.
   - `code`: Short error identifiers.
   - `hash`: Unique hashes for detailed server logs.
   - `client_msg`: Messages for end-users.
   - `server_msg`: Detailed messages for debugging.

## Internal Structure

The library consists of:

- **`error-code/lib.rs`**: Entry point defining the macro and exporting functionality.
- **`error-code-derive/error_info.rs`**: Implements the macro logic, using the `darling` library for parsing derive attributes and generating code.
- **`error_code_derive/lib.rs`**: Implements the `ToErrorInfo` procedural macro. Parse user-defined enums and generate the corresponding Rust code. It acts as the integration point for the macro logic defined in `error_info.rs`.

## Contributing

1. Fork the repository.
2. Create your feature branch: `git checkout -b feature-name`.
3. Commit your changes: `git commit -m 'Add feature'`.
4. Push to the branch: `git push origin feature-name`.
5. Open a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
