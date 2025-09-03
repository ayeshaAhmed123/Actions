FROM nginx:alpine

RUN chown -R nginx:nginx /var/cache/nginx && \
    chown -R nginx:nginx /etc/nginx && \
    chown -R nginx:nginx /var/log/nginx
RUN touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid
USER nginx
# Expose the port PHP will serve on
EXPOSE 80 
