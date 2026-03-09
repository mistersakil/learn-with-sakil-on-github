# Command line tools

## Queue commands



```buildContainer
1. Check with ps (most common). If queues are running using php artisan queue:work:
ps aux | grep "artisan queue:work" | grep -v grep | wc -l

2. Using pgrep (cleaner)
pgrep -af "artisan queue:work" | wc -l

3. If inside Docker container
docker exec -it container_name bash
then 
ps aux | grep queue

```