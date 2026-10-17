# Stratum

A single-file 3D explorer for understanding how a system is put together. Open it, pick an example architecture, and click around to see what each part does and what it depends on. You can also load your own system from a small JSON file, and an AI assistant can write that file for you.

## Open it

1. Save `stratum.html` anywhere on your computer.
2. Double-click it to open it in a browser (Chrome, Edge, Safari or Firefox).

You need an internet connection when it opens. It loads the 3D library and fonts from the web. Nothing you load or type is sent anywhere, and there is no account or install.

## Explore an architecture

- **Templates** opens 17 example systems (web apps, serverless, data pipelines and more). Pick one to load it.
- **Orbit and zoom:** drag to rotate. On a touch screen, use two fingers to move and pinch to zoom. The **Move**, **+** and **-** buttons on the right do the same.
- **Click a component** to see what it is, its group, who owns it, its tech stack, what it depends on, what uses it, and a chain of related components highlighted around it.
- **Click a connection** to see what flows between the two components and whether it is a request, an event or an optional path.
- **Search** finds a component by name.
- **More** has the component list, legend and group filters, text size, connection labels, spread, flow animation, fit and reset.
- The built-in tour (7 steps) shows you around the first time, including Move, Zoom in, Zoom out and Your own. Replay it any time from More > Walkthrough.

### Reading the connections

| Line | Meaning |
|---|---|
| Solid, request colour | One part calls another and waits for the answer |
| Event colour | Events or messages. Nobody waits for a reply |
| Dotted | Optional path, such as a cache |
| Alternate | A backup path |

## Make your own architecture

Press **Your own** in the toolbar and follow the steps in the window.

1. **Download example** (or **Copy example text**). This is the format the AI should copy.
2. **Describe your system** in a sentence or two, for example: "an online store with a mobile app, an API, a payments service, a product database and a queue for order emails".
3. **Copy prompt**, open any AI assistant (Claude, ChatGPT, Gemini), upload the example file or paste its text, then paste the prompt and send it.
4. **Copy the JSON** it gives back (only the part from `{` to `}`). Paste it into the box in Stratum, or save it as a `.json` file and use **Upload .json file**. Press **Load**.
5. **If you see an error,** paste the error message back to the AI and ask it to fix the JSON.

Notes:
- Fenced replies work. If the AI wraps the JSON in a code block or adds a sentence around it, Stratum pulls the JSON out.
- Connections that point at components that do not exist are skipped, and Stratum tells you which ones.
- Keep the file under 500 KB and the diagram under 150 components. 10 to 40 components reads best.

### JSON format

```json
{
  "name": "Simple web app",
  "layout": "flow",
  "groups": [{ "id": "app", "name": "Application" }],
  "nodes": [
    { "id": "api", "name": "API", "type": "service", "group": "app",
      "stack": "Node.js", "owner": "Backend team", "desc": "Handles every request." }
  ],
  "edges": [
    { "from": "browser", "to": "api", "kind": "sync", "label": "HTTPS requests" }
  ]
}
```

| Field | Values |
|---|---|
| `layout` | `flow` |
| `type` | client, phone, printer, sensor, edge, firewall, router, switch, loadbalancer, vpn, wifi, service, container, function, job, model, cache, queue, topic, database, storage, dataset, external |
| `kind` (connection) | `sync` request and response, `async` events or messages, `soft` optional, `alt` backup path |
| `flip` | Set `true` for a consumer reading from a queue or topic. Draw it from the consumer to the queue |

Every node needs a unique `id` and a `name`. Every connection's `from` and `to` must match a node `id`. Everything else is optional.

## Tips for good diagrams

- Group by layer or responsibility (users, edge, services, data) and keep to 3 to 8 groups.
- Draw each connection from the part that calls to the part that is called.
- Name the tech in `stack` and the team in `owner`. They show up when you click a component.
- Use one short sentence in `desc`.

## Good to know

- Diagrams are not saved in the page. To keep one, keep its JSON file and load it again later.
- Reloading the page returns to the default example.
- Tested in desktop Chrome and a simulated browser. Phones, Safari and Firefox have not been tested, so tell me what you find.
