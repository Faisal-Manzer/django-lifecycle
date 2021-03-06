# Available Hooks & Conditions

You can hook into one or more lifecycle moments by adding the `@hook` decorator to a model's method. The moment name is passed as the first positional argument, `@hook('before_create')`, and optional keyword arguments can be passed to set up conditions for when the method should fire.

## Decorator Signature

```python
    @hook(
        moment: str, 
        when: str = None, 
        when_any: List[str] = None, 
        has_changed: bool = None,
        is_now: Any = '*',
        is_not: Any = None,
        was: Any = '*', 
        was_not: Any = None,
        changes_to: Any = None,
    ):
```
## Lifecycle Moments

Below is a full list of hooks, in the same order in which they will get called during the respective operations:

| Hook name       | When it fires   |
|:-------------:|:-------------:|
| before_save | Immediately before `save` is called |
| after_save | Immediately after `save` is called
| before_create | Immediately before `save` is called, if `pk` is `None` |
| after_create | Immediately after `save` is called, if `pk` was initially `None` |
| before_update | Immediately before `save` is called, if `pk` is NOT `None` |
| after_update | Immediately after `save` is called, if `pk` was NOT `None` |
| before_delete | Immediately before `delete` is called |
| after_delete | Immediately after `delete` is called |


## Condition Keyward Arguments

If you do not use any conditional parameters, the hook will fire every time the lifecycle moment occurs. You can use the keyword arguments below to conditionally fire the method depending on the initial or current state of a model instance's fields.

| Keywarg arg       | Type   | Details |
|:-------------:|:-------------:|:-------------:|
| when | str | The name of the field that you want to check against; required for the conditions below to be checked. Use the name of a FK field to watch changes to the related model *reference* or use dot-notation to watch changes to the *values* of fields on related models, e.g. `"organization.name"`. But [please be aware](fk_changes.md#fk-hook-warning) of potential performance drawbacks. |
| when_any | List[str] | Similar to the `when` parameter, but takes a list of field names. The hooked method will fire if any of the corresponding fields meet the keyword conditions. Useful if you don't like stacking decorators. |
| has_changed | bool | Only fire the hooked method if the value of the `when` field has changed since the model was initialized  |
| is_now | any | Only fire the hooked method if the value of the `when` field is currently equal to this value; defaults to `*`.  |
| is_not | any | Only fire the hooked method if the value of the `when` field is NOT equal to this value  |
| was | any | Only fire the hooked method if the value of the `when` field was equal to this value when first initialized; defaults to `*`.  |
| was_not | any | Only fire the hooked method if the value of the `when` field was NOT equal to this value when first initialized. |
| changes_to | any | Only fire the hooked method if the value of the `when` field was NOT equal to this value when first initialized but is currently equal to this value. |