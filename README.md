# no-code-data-ingest
No-code data ingest for data pipelines.
You can find more find more info on https://medium.com/swlh/no-code-data-collect-api-f8e934ed8535?sk=7e70ed37a63068b615b3c8b93a832f70

## How To
1. Clone the repo
2. Perform chmod 755 on all .sh files
3. Run './run_all.sh'
4. Run curl localhost:8080/event?msg=testing no-code-data-ingest
5. Ensure that you can see your message in fluentd ouput
