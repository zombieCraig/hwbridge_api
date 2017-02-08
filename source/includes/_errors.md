# Errors

Default all unknown requests to return a JSON with status of not supported.

> All unhandled and unknown requests return "not supported"

```json
{
  "status": "not supported"
}
```

```javascript
// Catches all unknown responses and returns "not supported"
// 404 Response not necessary
app.get('*', function(req, res) {
  res.setHeader('Content-Type', 'application/json');
  res.status(404).send(JSON.stringify({ status: "not supported" }, null, 1));
});
```
