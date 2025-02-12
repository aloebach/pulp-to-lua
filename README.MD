# Pulp-To-Lua

*Converts [Pulp](https://play.date/pulp/) projects to the Lua SDK for the [Panic Playdate](https://play.date/).*

This transpiler is primarily intended to improve performance.

## Requirements

You must have [python 3](https://www.python.org/) installed, and the `PIL` or `Pillow` module as well. `pip install Pillow` on the command line ought to suffice.

## Usage

From your command line:

`python3 pulplua.py MyPulpProject.json out/`

You can then compile using `pdc` as normal:

```bash
    pdc out MyPulpProject.pdx
    PlaydateSimulator ./MyPulpProject.pdx
```

## Optimization Advice

While `pulp-to-lua` should already significantly improve performance by an order of magnitude at least, there are additional steps you can take to make sure your code runs optimally:

- Avoid using `emit` wherever possible. This function is slow and it is usually preferable to use `call` unless you really do want all actors (tile instances, player, room, game) to respond.
- There is generally no need to replace string literals for tile IDs e.g. `draw "white-tile"` for `draw 0`. The transpiler will do this automatically if it can detect this.
- To improve performance, up to around ~150 variables will be declared as `local` by the transpiler, and it will select the variables that appear most frequently in the code. This is not necessarily the variables which are most frequently used at runtime! Do with this information what you will -- you may wish to reuse variable names where possible.
- reading from `event.px`, `event.py`, `event.x`, `event.y` are a bit more expensive. Cache them at the start of the function if possible.
- `mimic` events can be heavily optimized by the transpiler for events that contain only one line and that line is `mimic` with a static name or id literal.

## Incompatabilities

- The transpilation is likely not perfect. Some behaviour may differ. You can help by reporting behaviourial differences.

## Contributions

Contributions are welcome! Ask the author (NaOH#1432 on [discord](https://discord.gg/VNVQHSS49U)) for advice.
