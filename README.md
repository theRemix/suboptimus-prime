# SubOptimus Prime

	primality	   contains the 3 prime number testers
	suboptimus	 the test runner

## Ideas

- FIFOs (named pipe)
    - easy split, 3 named sockets
    - increase number of sockets to utilize all cores?
- unix sockets
    - network sockets? scalable number of workers
        - capped by max socket limit
        - not fault tolerant
- message queues
    - will have to deal with writing results somewhere, will order matter?
    - scalable number of workers
    - not capped by socket limit
    - fault tolerant

### FIFO

- bash script: run-fifo.sh
    - configure concurrency count and params
    - exec suboptimus with params [concurrency]times in bg
        - creates named pipe
        - wait for readers
        - send test jobs
    - exec primality [concurrency]times in bg
        - write results to report
    - wait for sub processes to end

*Questions*

- single or multiple suboptimus?

### Unix Sockets

- systemd unit: primality@n
    - launch primality worker
- start primality services x [concurrency]
    - opens unix socket [id]
    - waiting for jobs
- bash script: suboptimus-test.sh
    - configure concurrency count and params
    - exec suboptimus with params [concurrency]times in bg
        - send test jobs by writing to unix socket [id]
        - read from same socket
        - write results to report
    - wait for sub processes to end

## Implementation

- [ ] Unix Sockets
