## vex2snap

Convert future scans(*) from a VLBI VEX file<sup id="vexref">[1](#vex)</sup> to `.snp`, `.prc` SNAP and procedure files for the [FieldSystem](https://github.com/nvi-inc/fs), replacing DRUDGE for a RDBE PFB/DDC + Mark5C/Mark6 running [jive5ab](https://github.com/jive-vlbi/jive5ab) combo.

(*) This is default behaviour, which can be overriden, see below

## Usage
```bash
  $> vex2snap [options] --station XX [vex1 [vex2 [...]]
 ```
See `vex2snap --help` for [options]                

`vex2snap` reads the vex file(s) and extracts the useful information for the station indicated by `XX`. Setups are generated for all modes found for station `XX`. Based on that information a `.prc` file with initialization and setup procedures is created. A `.snp` SNAP file containing the actions for the whole experiment is also output.

Input of <b>xxx.vex</b> produces <b>/usr2/sched/xxx.{prc,snp}</b>  [the default output path]

Note: if the output file(s) already exist, the program halts. This behaviour can be overridden through the `--force` option.

## Options


    --help             show short help
    --usage            show this built-in help
    -v, --version      print version and exit succesfully

    --station          Two-letter station code to generate .prc/.snp for

    --dest             set destination path for output files

    --shift today | [+-]<vextime>
                       shift all UTC time stamps in the schedule.
                       Supported OFFSET formats:
                       'today'       shift day of observation to today.
                                     Uses a sidereal day length of 23h56m4s
                                     so the elevation of the sources
                                     should be retained
                       <vexime>      shift to day-of-year in time stamp,
                                     see 'today'
                       [+-]<vextime> add/subtract this much to each time stamp


     --recorder TYPE[:DISKS]
                       Override scheduled recorder from VEX file 
                       By default vex2snap will use the recorder defined
                       in the VEX file for the selected station.
                       Override by using this option.
                       TYPE  = Mark5C | Mark6 | FlexBuff | eVLBI | none
                       DISKS = (only if TYPE=Mark6 | FlexBuff):
                               argument to "set_disks=", specify on which
                               disks or module to record the experiment.
                       Default for
                         TYPE=Mark6 "mk6",
                         TYPE=FlexBuff "flexbuff"
                       i.e. all presently mounted disks on the
                       indicated harware.

                       To use a Mark6 as FlexBuff use this:
                            --recorder FlexBuff:mk6 
                       which will record in FlexBuff format on Mark6 disks

    --force            overwrite output files [default: don't!]

    --ignore-utc       do not forget scans from the past [default:
                       only process scheduled scans that are still
                       observable - i.e. in the future]

    --postob-duration  if you have an estimate how long your postob+checkmk5
                       procedures will take you can override the default
                       assumption of 20s that vex2snap uses to decide
                       whether there is enough time between two scans
                       to insert those commands                                                  


<b id="vex">1</b> https://vlbi.org/vlbi-standards/vex/ [↩](#vexref)
