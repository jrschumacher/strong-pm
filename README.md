# strong-deploy

Config notes.

Config is an .ini file. Configurable are:

- prepare command: defaults to `npm rebuild; npm install --production`
- start command: defaults to `sl-run` XXX should default be clustered?
- stop signal: defaults to `SIGTERM`

Configuration can be global, or per repo pushed to.

Push from git with a command like `git push http://localhost:PORT/REPO`

PORT is arg to --listen, REPO you decide but is not optional

Example:

    ; these are the defaults:
    prepare[] = npm rebuild
    prepare[] = npm install --production
    start = sl-run
    stop = SIGTERM

    ; these are overrides for a particular repo
    [config-one]
    ; no prepare
    prepare =
    start = node .
    stop = SIGHUP


Architecture:

        ------
        node
          cluster | net | ...
        ------------------
        strong-   |
        cluster-  |
        control   |
        ------------------------
        strong-
        supervisor
           uses: fs, syslog, run-time control, strong-agent, s-c-c, signals, ...
        
           XXX the control channel/pipe should not be in cluster-control!
        ---------------------------------
            upstart      |     strong-deploy    
        how to restart   |   git receives, and SIGHUPs to trigger chdir/and
        becomes your     |   worker restart
        problem, but not |
        a problem if     |
        you deploy in
        containers
