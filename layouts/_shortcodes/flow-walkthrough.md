{{/* Text equivalent of flow-walkthrough.html. Keep the intro and both ordered
  step lists in sync with the HTML template: agents must receive the complete
  explanation without JavaScript or the interactive diagram. */ -}}
### Compare the two outcomes

The same configured request tries the primary first. Only a retryable primary status takes the fallback path.

#### Primary succeeds

1. The client sends an OpenAI Responses request to the configured listener.
2. The outer format and validation filters classify the request and prepare its state.
3. The primary step classifies and validates again, translates the body to Chat Completions, and rewrites `/v1/responses` to `/v1/chat/completions`.
4. The router selects `primary-backend`. Credential injection removes a client credential and adds that backend's configured credential; the load balancer selects the primary endpoint.
5. The response is translated back to the Responses API shape and returned to the client. The fallback step does not run.

#### Primary returns a retryable status

1. The client sends an OpenAI Responses request to the configured listener.
2. The outer format and validation filters classify the request and prepare its state.
3. The primary step reclassifies and validates, translates to Chat Completions, rewrites the path, and routes to the primary backend with its configured credential.
4. A primary status of 429, 502, 503, or 504 selects the fallback step.
5. The fallback step rebuilds its format and validation state, translates the request, and rewrites the path again.
6. The router selects `fallback-backend`; credential injection uses that backend's configured credential.
7. The load balancer selects the fallback endpoint.
8. The fallback response is translated back to the Responses API shape and returned to the client.
