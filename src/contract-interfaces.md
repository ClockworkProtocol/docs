# Contract Interfaces

To be able to use Clockwork's core capabilities, users must first implement Clockwork Interface in their smart contract.

## Import interface message

In `Cargo.toml` file, Add Clockwork Interface package to the dependencies section.

```toml
[dependencies]
interface = { git = "https://github.com/ClockworkProtocol/clockwork-contracts" }
# ...
```

## Add Clockwork variant

Add Clockwork's message variant to the main contract execute message and query message enum.

```rust
#[serde(rename_all = "snake_case")]
pub enum InterfaceExecuteMsg {
    Clockwork(ClockworkExecuteMsg), // <- Add this line
    // ...
    // Other contract-specific messages, ex.
    // Deposit {},
    // ...
}

#[serde(rename_all = "snake_case")]
pub enum InterfaceQueryMsg {
    Clockwork(ClockworkQueryMsg), // <- Add this line
    // ...
    // Other contract-specific messages, ex.
    // Config {},
    // ...
}
```

## Handle message logic

In `contract.rs` or the entry point handler file, add enum matching for handling Clockwork-specific messages.

```rust
pub fn execute(
    _deps: DepsMut,
    _env: Env,
    _info: MessageInfo,
    msg: ExecuteMsg,
) -> Result<Response, ContractError> {
    match msg {
        ExecuteMsg::Increment {} => try_increment(deps),
        ExecuteMsg::Clockwork(clockwork_msg) => match clockwork_msg {
            ClockworkExecuteMsg::Execute { .. } => try_clockwork_increment(deps),
        },
        // ...
        // Other contract-specific messages handler, ex.
        // QueryMsg::GetCount {} => to_binary(&query_count(deps)?),
    }
}

pub fn query(_deps: Deps, _env: Env, msg: QueryMsg) -> StdResult<Binary> {
    match msg {
        QueryMsg::Clockwork(clockwork_msg) => match clockwork_msg {
            ClockworkQueryMsg::Check => todo!(),
        },
        // ...
        // Other contract-specific messages handler, ex.
        // QueryMsg::GetCount {} => to_binary(&query_count(deps)?),
    }
}
```
