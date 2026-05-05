# ComfyUI Workflow Variables

Custom ComfyUI workflows use API-format JSON from ComfyUI's `Save (API Format)` export. Paste that JSON into `Custom Workflow JSON`.

QIG replaces placeholders anywhere inside string values before sending the workflow to `/prompt`.

| Placeholder | Value |
| --- | --- |
| `%prompt%` | Final positive prompt after LLM prompt generation, style, quality tags, ST Style, contextual filters, and wildcard expansion. |
| `%negative%` | Final negative prompt after ST Style, contextual filters, and wildcard expansion. |
| `%seed%` | Resolved seed for this image. If the seed is `-1`, QIG resolves a random seed before sending the workflow. |
| `%width%` | Current width setting. |
| `%height%` | Current height setting. |
| `%steps%` | Current steps setting. |
| `%cfg%` | Current CFG scale setting. |
| `%denoise%` | Current ComfyUI denoise setting. |
| `%clip_skip%` | Current ComfyUI clip skip setting. |
| `%sampler%` | ComfyUI sampler name derived from the selected sampler. |
| `%scheduler%` | Current ComfyUI scheduler setting. |
| `%model%` | Current Local Model field. |
| `%reference_image%` | Uploaded ComfyUI image filename for the Local reference image, or an empty string when no reference image is set. |

## Typed Values

When a node input is exactly a placeholder, QIG preserves numeric types where possible:

```json
"seed": "%seed%",
"width": "%width%",
"cfg": "%cfg%"
```

Those become numbers in the submitted workflow. If a placeholder is embedded inside other text, it is replaced as text:

```json
"filename_prefix": "qig_%seed%"
```

## Reference Images

To use the Local reference image in a custom workflow:

1. Add a `LoadImage` node in ComfyUI.
2. Export the workflow in API format.
3. Set that node's `image` input to `%reference_image%`.

If no Local reference image is selected, `%reference_image%` becomes an empty string.

## Notes

- Placeholders are replaced only in JSON string values.
- Use API-format JSON, not the regular visual workflow export.
- If the custom workflow JSON cannot be parsed, QIG falls back to its built-in default workflow.
- Runtime ComfyUI or proxy JSON errors are reported as runtime errors instead of being relabeled as invalid workflow JSON.
