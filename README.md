`ssh deploy@1440417-cl95133.tw1.ru`

```sh
export CURRENT_DOMAIN=domain.ru
```

```sh
create_site $CURRENT_DOMAIN && \
echo $CURRENT_DOMAIN >> domain-list.txt && \
cd docker-compose/ && \
docker compose up -d --build
```

```sh
docker exec acme.sh --issue -d $CURRENT_DOMAIN -d "*.$(echo $CURRENT_DOMAIN)" \
  --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please \
  --server letsencrypt \
  --keylength ec-256
```

```
curl -X POST -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TIMEWEB_CLOUD_TOKEN" \
  -d '{"type":"TXT","subdomain":"_acme-challenge","value":"PASTE-TXT-HERE"}' \
  "https://api.timeweb.cloud/api/v1/domains/$CURRENT_DOMAIN/dns-records"
```
