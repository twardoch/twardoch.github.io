# Adam Twardoch

Public repositories:

{% for repo in site.github.public_repositories %}
[{{ repo.full_name }}]({{ repo.html_url }})
: {{ repo.description }}
{% endfor %}

