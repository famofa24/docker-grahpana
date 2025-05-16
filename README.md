
  <h1>grafana-docker-stack</h1>

  <p>For deploying Grafana, Prometheus and Node Exporter, make these steps:</p>

  <h3>1. Deploy stack</h3>
  <pre><code>git clone https://github.com/digitalstudium/grafana-docker-stack.git
docker stack deploy -c grafana-docker-stack/docker-compose.yml monitoring</code></pre>

  <h3>2. Add prometheus datasource</h3>
  <p>With address <code>http://prometheus:9090</code> to grafana</p>

  <h3>3. Add these three lines to the bottom of <code>/var/lib/docker/volumes/monitoring_prom-configs/_data/prometheus.yml</code> file, to <code>scrape_configs:</code> section:</h3>
  <pre><code>- job_name: 'node-exporter'

  static_configs:
    - targets: ['node-exporter:9100']</code></pre>

  <h3>4. Reload prometheus config via this command:</h3>
  <pre><code>docker ps | grep prometheus | awk '{print $1}' | xargs docker kill -s SIGHUP</code></pre>

  <h3>5. Import this dashboard:</h3>
  <p><a href="https://grafana.com/grafana/dashboards/1860" target="_blank">https://grafana.com/grafana/dashboards/1860</a> to grafana.</p>

  <p>That's it!</p>

  <hr>

  <h3>If you want to add more servers to prometheus, make these steps:</h3>

  <h4>1. Install node-exporter to each of these servers via these commands:</h4>
  <pre><code>git clone https://github.com/digitalstudium/grafana-docker-stack.git
docker stack deploy -c grafana-docker-stack/node-exporter.yml node-exporter</code></pre>

  <h4>2. Add these servers to <code>/var/lib/docker/volumes/monitoring_prom-configs/_data/prometheus.yml</code> file to <code>- targets: ['node-exporter:9100']</code> list of <code>- job_name: 'node-exporter'</code> section, like:</h4>
  <pre><code>- targets: ['node-exporter:9100', 'server1:9100', 'server2:9100', '...']</code></pre>

  <h4>3. Reload prometheus config via this command:</h4>
  <pre><code>docker ps | grep prometheus | awk '{print $1}' | xargs docker kill -s SIGHUP</code></pre>

</body>
</html>
