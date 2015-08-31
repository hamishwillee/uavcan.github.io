{% if include.header_prefix == null %} {% assign HEADER_PREFIX = '###' %}
{% else %}                             {% assign HEADER_PREFIX = include.header_prefix %}
{% endif %}
{% if include.prefix == null %} {% assign PREFIX = 'uavcan' %}
{% else %}                      {% assign PREFIX = include.prefix %}
{% endif %}

{% assign dsdl_files = site.static_files | where: 'extname', '.uavcan' | sort: 'path' %}

{% for x in dsdl_files %}
    {% include dsdl_parse_path.md input_path=x.path %}
    {% if FULL_TYPE_NAME contains PREFIX %}
        {% if include.split_by_namespace and NAMESPACE != prev_namespace %}
{{HEADER_PREFIX | remove_first: '#'}} `{{NAMESPACE}}`
            {% assign prev_namespace = NAMESPACE %}
        {% endif %}
        {% if include.split_by_namespace %}
{{HEADER_PREFIX}} `{{TYPE_NAME}}`
Full name: `{{FULL_TYPE_NAME}}`
        {% else %}
{{HEADER_PREFIX}} `{{FULL_TYPE_NAME}}`
        {% endif %}
        {% if DEFAULT_DTID != null %}
Default data type ID: {{DEFAULT_DTID}}
        {% endif %}
```python
{% include_relative {{RELATIVE_PATH}} %}
```
    {% endif %}
{% endfor %}