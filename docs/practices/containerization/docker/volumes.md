# Volumes

If you have a container that is writing logs, for example, you may want to persist that. So, you can put these log files somewhere else.

`docker run -p <ports> -v $(pwd)/var/www/logs <imageToRun>`