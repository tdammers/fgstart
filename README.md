# fgstart

A CLI launcher for FlightGear.

# Copying / License

Copyright (c) 2022 Tobias Dammers.

See attached COPYING file for license information.

SPDX: MIT

# Installation & Configuration

FGStart is a self-contained shell script; put it anywhere on your `$PATH`, and
it should work.

You will need the following external programs:

- FlightGear (`fgfs`) (duh).
- `bash` and the usual suspects that go with it
- `xmlstarlet`
- `curl` (optional, for SimBrief integration)
- `gdb` (optional, for debugging FG core)

FGStart will most likely not work on Windows, and has only been tested on
GNU/Linux. It may or may not work on OS X.

# Configuration

- Create a directory `$HOME/.config/fgstart`
- Copy the provided `settings` file into `$HOME/.config/fgstart/settings` and
  edit to taste

## Fleet Configuration
To use the fleet features (`--type`, `--airframe`) and/or the SimBrief
integration (`--simbrief`), you need to create a file hierarchy in
`$HOME/.config/fgstart/fleet`, with two subdirectories:

- `types`, which contains one file per aircraft type you want to use. The
  filename can be anything you want, but for the SimBrief integration feature,
  it must match the aircraft type's ICAO code (e.g. `A20N` for the A320neo).
  The filename is what you pass to the `--type` option, and the `--simbrief`
  option uses it to match aircraft types for which you don't have an airframe
  defined (see below).
- `airframes`, containing one file per airframe you want to use. Again, the
  filename can be anything you want, but for the SimBrief integration feature,
  it should match the tail number / registration from the SimBrief "Fleet"
  page, exactly as found there (case sensitive, and with the dashes included or
  omitted exactly as in SimBrief). The filename can be passed to the
  `--airframe` option, or matched by the `--simbrief` option.

These files are shell scripts, and they can do anything you want, but
typically, they should only set two or three environment variables, namely:

- `AIRCRAFT`: the aircraft type as passed to `--aircraft`; this is the exact
  name that `fgfs` uses, not the ICAO type.
- `ACTYPE`: the aircraft type as passed to `--type` (usually the ICAO type).
- `AIRFRAME`: the airframe as passed to `--airframe` (usually the tail number).
- `CALLSIGN`: ATC callsign. You should only set this for airframes you intend
  to fly without an operator callsign.

# Usage

Basic usage: just call `fgstart`, and it will interactively query for the
parameters it needs (aircraft type, airport, parking position or runway).

Available options:

- `--fgfs BINARY` - use a different `fgfs` binary than the default one (e.g.
  `/usr/local/bin/fgfs-2020.3.1`).
- `--dump` - instead of executing the `fgfs` command, dump it to the console.
- `--show-aircraft` - instead of running `fgfs` normally, just make it dump
  a list of available aircraft types.
- `--airport AIRPORT` - set the airport to spawn at
- `--aircraft AIRCRAFT` - select aircraft to load (FGFS name, as per `--show-aircraft`)
- `--airframe AIRFRAME` - load airframe by tail number (from
  `~/.config/fgstart/fleet/airframes/$AIRFRAME`)
- `--type ACTYPE` - load aircraft type (from
  `~/.config/fgstart/fleet/types/$ACTYPE`)
- `--simbrief` - download your current flight plan from simbrief, and fill in
  airport, callsign, airframe, and aircraft type based on the flight plan
- `--parkpos PARKPOS` - select a parking position to spawn at
- `--runway` - select a runway to spawn on
- `--scenery SCENERY` - select a scenery from the following options:
  - `terragit` - use TerraGIT scenery from the configured path
  - `terrasync` - use TerraSync and enable automatic scenery downloads
  - `terrasync-existing` - use Terrasync, but don't enable automatic downloads
    (only use previously downloaded Terrasync scenery)
  - `1.0` - use WS1.0
  - `none` - don't load any default scenery (only use extra scenery)
- `--photoscenery` - use configured photoscenery
- `--hdr` - use the (experimental) HDR pipeline
- `--time TIME` - set sim time
- `--callsign CALLSIGN` - select an ATC callsign
- `--mpserver NUMBER` - if given, connect to multiplayer server `$NUMBER`
- `--multimon` - load multi-monitor configuration (as per settings)
- `--singlethreaded` - run single-threaded
- `--multithreaded` - use normal multithreading configuration
- `--multithreaded2` - use aggressive multithreading configuration (tends to
  render FGFS unstable)
- `--antialias MODE` - select anti-aliasing mode from:
  - `2`, `3`, `4`: use FG's built-in anti-aliasing (2x / 3x / 4x)
  - `off`: no anti-aliasing
  - `lo`, `med`, `hi`: use environment variables to configure the NVidia driver
    for various anti-aliasing and anisotropic filtering options.
- `--extra-scenery` - use extra scenery as per settings
- `--no-extra-scenery` - disable extra scenery, use only scenery as per
  `--scenery`
- `--addons` - enable addons as per settings
- `--no-addons` - run without addons
- `--rembrandt` - deprecated; enable Rembrandt
- `--compositor` - deprecated; enable Compositor (always on in current FG)
- `--log-level` - select a logging level
- `--gdb` - run `fgfs` through `gdb`
- `--headtrack` - configure `fgfs` for OpenTrack (experimental)
- `--telnet` - open telnet connection on `localhost:10001`, and allow external
  Nasal commands
