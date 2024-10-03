# Rust_Learning
Record Issues when using Rust &amp; Dioxus

## Can not init complicate value in #[props(default = **)]
#[props(default)]  can not used to init complicate type: Signal etc. 


## Multiple move will cause code not execute
This code won't excute after wait, can not output "login succ" & "in async move error"
```
    let mut is_logining = use_signal(|| false);
    let perform_login = move || {
        info!("in perform_login");
            info!("in perform_login future");
            async move {
                let mut user_state = user_state.clone();
                info!("in async move");
                match login(email).await {
                    Ok(user) => {
                        info!("login succ");
                    }
                    Err(e) => {
                         info!("in async move error");
                    }
                }
            }
    }
    let on_click = move |_| {
        is_logining.set(true);
        perform_login();
    }
    ***
    rsx!{
        button {
            on_click,
            disable: *is_login.read()
        }
    }
```
