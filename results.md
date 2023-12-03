
# Results

## pgbench

Direct connection to PostgreSQL server.

```console
pgbench --builtin simple-update --client $(expr $(psql $PGDATABASE -c 'show max_connections' -t -A) / 4) --jobs $(getconf _NPROCESSORS_ONLN) --progress 10 --protocol prepared --time 180

pgbench (15.5 (Homebrew), server 15.1 (Ubuntu 15.1-1.pgdg20.04+1))
starting vacuum...end.
progress: 10.0 s, 257.7 tps, lat 85.354 ms stddev 106.396, 0 failed
progress: 20.0 s, 299.1 tps, lat 83.280 ms stddev 109.327, 0 failed
progress: 30.0 s, 316.0 tps, lat 79.511 ms stddev 97.353, 0 failed
progress: 40.0 s, 312.5 tps, lat 79.624 ms stddev 101.112, 0 failed
progress: 50.0 s, 284.6 tps, lat 88.235 ms stddev 106.307, 0 failed
progress: 60.0 s, 270.0 tps, lat 92.582 ms stddev 113.800, 0 failed
progress: 70.0 s, 284.3 tps, lat 87.637 ms stddev 110.206, 0 failed
progress: 80.0 s, 294.9 tps, lat 85.072 ms stddev 106.125, 0 failed
progress: 90.0 s, 282.0 tps, lat 88.665 ms stddev 110.655, 0 failed
progress: 100.0 s, 311.5 tps, lat 80.249 ms stddev 101.873, 0 failed
progress: 110.0 s, 315.5 tps, lat 79.019 ms stddev 100.141, 0 failed
progress: 120.0 s, 308.4 tps, lat 81.438 ms stddev 99.573, 0 failed
progress: 130.0 s, 288.0 tps, lat 86.494 ms stddev 110.750, 0 failed
progress: 140.0 s, 297.7 tps, lat 83.419 ms stddev 107.713, 0 failed
progress: 150.0 s, 296.7 tps, lat 84.872 ms stddev 110.682, 0 failed
progress: 160.0 s, 310.6 tps, lat 80.410 ms stddev 103.618, 0 failed
progress: 170.0 s, 310.1 tps, lat 80.891 ms stddev 102.104, 0 failed
progress: 180.0 s, 319.6 tps, lat 77.979 ms stddev 95.488, 0 failed
transaction type: <builtin: simple update>
scaling factor: 200
query mode: prepared
number of clients: 25
number of threads: 16
maximum number of tries: 1
duration: 180 s
number of transactions actually processed: 53618
number of failed transactions: 0 (0.000%)
latency average = 83.457 ms
latency stddev = 105.199 ms
initial connection time = 1097.326 ms
tps = 299.028596 (without initial connection time)
```

## simple-update

```console
psql $PGDATABASE -c 'set statement_timeout = 0' -c 'truncate pgbench_history' -c 'vacuum analyze' && k6 run simple-update.js
          /\      |‾‾| /‾‾/   /‾‾/
     /\  /  \     |  |/  /   /  /
    /  \/    \    |     (   /   ‾‾\
   /          \   |  |\  \ |  (‾)  |
  / __________ \  |__| \__\ \_____/ .io

  execution: local
     script: simple-update.js
     output: -

  scenarios: (100.00%) 1 scenario, 500 max VUs, 3m30s max duration (incl. graceful stop):
           * default: 500 looping VUs for 3m0s (gracefulStop: 30s)


     data_received..................: 11 MB  62 kB/s
     data_sent......................: 13 MB  73 kB/s
     http_req_blocked...............: avg=3.86ms   min=0s       med=0s       max=4.34s   p(90)=1µs    p(95)=1µs
     http_req_connecting............: avg=340.22µs min=0s       med=0s       max=1.01s   p(90)=0s     p(95)=0s
     http_req_duration..............: avg=825.41ms min=199.34ms med=701.84ms max=2.99s   p(90)=1.25s  p(95)=1.36s
       { expected_response:true }...: avg=825.41ms min=199.34ms med=701.84ms max=2.99s   p(90)=1.25s  p(95)=1.36s
     http_req_failed................: 0.00%  ✓ 0          ✗ 108716
     http_req_receiving.............: avg=893.79µs min=4µs      med=44µs     max=25.59ms p(90)=3.26ms p(95)=4.78ms
     http_req_sending...............: avg=108.24µs min=8µs      med=57µs     max=18.65ms p(90)=164µs  p(95)=243µs
     http_req_tls_handshaking.......: avg=3.46ms   min=0s       med=0s       max=4.27s   p(90)=0s     p(95)=0s
     http_req_waiting...............: avg=824.41ms min=199.28ms med=700.89ms max=2.99s   p(90)=1.25s  p(95)=1.36s
     http_reqs......................: 108716 602.006831/s
     iteration_duration.............: avg=829.5ms  min=290.92ms med=702.39ms max=5.42s   p(90)=1.25s  p(95)=1.38s
     iterations.....................: 108716 602.006831/s
     vus............................: 500    min=500      max=500
     vus_max........................: 500    min=500      max=500
```
