# Flink users/Test consolidation

This first pipeline reads JSON records from Kafka topics `users` and `Test`,
joins them on `id`, selects the required user and test columns, and writes the
result to ClickHouse table `fdp.consolidated_users`.

Start the infrastructure with `docker compose up -d --build`. Submit the SQL
job from the Flink SQL client/container using `flink/sql/consolidate_users.sql`,
then run `python Producer/publish_csv_topics.py --limit 1000` from the host.

The join is an inner streaming join. A record is emitted when matching IDs are
available on both Kafka topics. Kafka retains the source events, so the job can
be restarted using its consumer groups and checkpoints.
