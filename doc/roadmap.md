# Roadmap

What each release contains is in the [changelog](../CHANGELOG.md); this page is about
what comes next.

## Planned

### Following plumbob

plumbob-async drives plumbob's I/O-free training core, so it follows plumbob's planned
changes (see plumbob's roadmap): the additive entry point that resumes training at LTS:3
from an active FRL link, and hdmi-hal-async's split of its PHY trait by link mode
(`HdmiPhy` and `SendHdmiPhy` renamed `FrlPhy` and `SendFrlPhy`, with
`set_frl_rate(NotSupported)` meaning "FRL off"), in a breaking release after
hdmi-hal-async's.

### The link orchestrator's async trainer interface

The planned link orchestrator's async companion will define the interface it drives
training through, with a hand-written `Send` twin as this crate's `SendScdcClient` has, and
implement it for plumbob-async in its own crate, behind a feature. Nothing in plumbob-async
changes for it.
