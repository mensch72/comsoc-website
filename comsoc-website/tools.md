---
layout: default
title: Tools | COMSOC
description: >
  Discover the tools that have been designed by Computational Social Choice (COMSOC) researchers.
---

<section markdown="1" class="section-with-navs">

# Tools for COMSOC

COMSOC researchers have developed a large number of tools over the years. 
These tools serve different objectives. 
Some are meant to help people produce better research.
Others are intended for outreach, to bring the benefits and excitement of COMSOC research to a broader audience.

Below you will find a list of such tools, organised by topic of study and by type of tool.
Badges are used to indicate relevant features:
- <i class="fa-solid fa-laptop-code"></i> indicates that the tool is open-source (click on the icon to view the sources);
- <i class="fa-solid fa-list-check"></i> indicates that the tool has been tested (mostly relevant for code libraries);
- <i class="fa-solid fa-book"></i> indicates that the tool has been documented (click on the icon to view the documentation).

For each topic, we have identified some "reference tools". These are tools that are particularly
helpful and that should to be considered by anyone who is thinking of developing new tools on the same topic.
Before developing a new tool, explore the reference tools on the topic to assess what the
conventions are (data format, etc.). Consider extending the reference tools rather than creating new ones.

Each tool also is assigned a tag based on where in the typical pipeline of a COMSOC problem it is most useful:
- <span class="badge tool-category">Preferences</span> tools to deal with data on preferences  

- <span class="badge tool-category">Aggregation</span> tools to perform aggregation of individual preferences into collective decisions

- <span class="badge tool-category">Analysis</span> tools for the analysis of aggregation processes and their outcomes

- <span class="badge tool-category">Application</span> application platforms offering COMSOC tools to the general public

- <span class="badge tool-category">Outreach</span> tools designed for outreach purposes

{% assign tools_per_topic = site.data.tools | group_by: "topic" %}

<div class="page-navigation-wrap">
<div class="page-navigation">
<span><a href="#best-practices">Best Practices</a></span>
{% for topic in site.tools_topics %}
{% assign tools = tools_per_topic | where: "name", topic %}
{% if tools.size > 0 %}
<span><a href="#{{ topic }}">{{ topic }}</a></span>
{% endif %}
{% endfor %}
</div>
</div>

</section>

<section id="best-practices" markdown="1">

## Best Practices

The development of tools for and by the COMSOC community is guided by the general spirit of this [community]({{ "community" | relative_url }})
together with the principles of [open-source development](https://opensource.com/resources/what-open-source){:target="_blank" rel="noreferrer"}.
We want to create a vibrant and inclusive ecosystem that benefits everyone.
The success of the tools can only come together with support from the community.

To this end, we have assembled a list of best practices to be considered when developing tools:
- **Assess the need**: A lot of tools have been developed already. It is often better to improve existing tools rather than to create new ones. Explore the tools below to get an idea of what exists and what the standards are. 
- **Maintainability**: Tools should be developed for the long term. Keep in mind that the initial developer may not be available 10 years down the line. Documentation is key.
- **Easy adoption**: Tools are successful only if they are easy to use. Documentation is key once again. Provide examples on how to use them.
- **Interoperability**: The tools for COMSOC form an ecosystem. When developing a new tool, think about its interoperability with other tools. Use standard data formats (such as the [PrefLib](https://www.preflib.org/format){:target="_blank" rel="noreferrer"} and the [Pabulib](https://pabulib.org/format){:target="_blank" rel="noreferrer"} format).
- **Reliability**: Confidence in the tool is crucial for it to be used by others. Demonstrate that your tool does what it should do. Tests are not overrated. As a first step, use all relevant examples from papers and textbooks as tests.
- **Visibility**: Ensure that your tools are visible on the platforms used by the community. Consider adding them to this list ([how to]({{ site.github_url}}){:target="_blank" rel="noreferrer"}) and archiving them in the [COMSOC repository on Zenodo](https://zenodo.org/communities/comsoc){:target="_blank" rel="noreferrer"} (you can easily archive GitHub repositories on Zenodo by [linking your account](https://help.zenodo.org/docs/profile/linking-accounts/){:target="_blank" rel="noreferrer"}).
 
Below is a list of programming languages together with the number of tools on this page that use them. 
This can be helpful in determining what might be a suitable programming languages for a future tool.

{% assign all_languages = "" | split: "" %}
{% assign all_languages_unique = "" | split: "" %}
{% for tool in site.data.tools %}
{% if tool.languages %}
{% for lang in tool.languages %}
{% assign all_languages = all_languages | push: lang %}
{% if all_languages_unique contains lang %}
{% else %}
{% assign all_languages_unique = all_languages_unique | push: lang %}
{% endif %}
{% endfor %}
{% endif %}
{% endfor %}

{% assign all_languages_unique = all_languages_unique | sort %}

<div class="programming-languages-wrap">
{% for unique_lang in all_languages_unique %}
{% assign count = 0 %}
{% for lang in all_languages %}
{% if unique_lang == lang %}
{% assign count = count | plus: 1 %}
{% endif %}
{% endfor %}
<div class="programming-languages"><span>{{ unique_lang }}:</span><span>{{ count }}</span></div>
{% endfor %}
</div>

Tools are often used to conduct numerical experiments. Consult this recent
[survey paper](https://arxiv.org/abs/2402.11765){:target="_blank" rel="noreferrer"}
for advice on how to run such experiments as well as a discussion of current standards and practices
(see also the associated [database](https://guide.cbip.matinf.uj.edu.pl/articles/){:target="_blank" rel="noreferrer"}
of experimental papers in COMSOC).

</section>

{% for topic in site.tools_topics %}
{% assign tools = tools_per_topic | where: "name", topic | first %}

{% if tools.items.size > 0 %}

<section id="{{ topic }}">

<h2>{{ topic }}</h2>

{% assign tools_sorted = tools.items | sort: "name" %}
{% assign reference_tools = tools_sorted | where: "reference-tool", true %}
{% assign other_tools = tools_sorted | where: "reference-tool", false %}

{% assign tool_types = "reference,other" | split: ',' %}

{% for tool_type in tool_types %}

{% if tool_type == "reference" %}
{% assign current_tools = reference_tools %}
{% else %}
{% assign current_tools = other_tools %}
{% endif %}

{% if current_tools.size > 0 %}
{% if reference_tools.size > 0 %}
<h3>{{ tool_type | capitalize }} Tools</h3>
{% endif %}

<div class="{{ tool_type }}-tools-wrap tools-wrap">

{% assign current_tools = current_tools | sort: "category" %}
{% for tool in current_tools %}
<div class="{{ tool_type }}-tool-wrap tool-wrap">

<h4 class="{{ tool_type }}-tool-title">
    <span>
        <a href="{{ tool.url }}" target="_blank" rel="noreferrer">
            {% if tool.type == "Website" %}
                <i class="fa-solid fa-globe"></i>
            {% elsif tool.type == "Python" %}
                <i class="fa-brands fa-python"></i>
            {% endif %}
            {{ tool.name }}
        </a>
    </span>
    <span class="badges-wrap">
        {% if tool.source-url %}<span><a href="{{ tool.source-url }}" title="To the source" target="_blank" rel="noreferrer"><i class="fa-solid fa-laptop-code"></i></a></span>{% endif %}
        {% if tool.has-tests %}<span title="Has been duly tested"><i class="fa-solid fa-list-check"></i></span>{% endif %}
        {% if tool.documentation-url %}<span><a href="{{ tool.documentation-url }}" title="To the documentation" target="_blank" rel="noreferrer"><i class="fa-solid fa-book"></i></a></span>{% endif %}
        {% if tool.category %}<span class="badge tool-category">{{ tool.category }}</span>{% endif %}
        {% if tool_type == "other"%}<span class="toggle-arrow">&#9662;</span>{% endif %}
    </span>
</h4>

<div class="{{ tool_type }}-tool-details tool-details">

<p>{{ tool.description }}</p>

<p>
    {% for contrib in tool.contributors %}<span{% if contrib.maintainer %} class="tool-contributor" title="Maintainer"{% endif%}>{% if contrib.url %}<a href="{{ contrib.url }}">{% endif %}{{ contrib.name }}{% if contrib.url %}</a>{% endif %}{% if contrib.maintainer %} (<i class="fa-solid fa-wrench maintainer-badge"></i>){% endif %}</span>{% unless forloop.last %}{% if forloop.rindex == 2 %} and {% else %}, {% endif %}{% endunless %}{% endfor %}
</p>

{% if tool.languages %}
<p class="programming-languages">
    {% for language in tool.languages %}<span>{{ language }}</span>{% endfor %}
</p>
{% endif %}
</div>
</div>
{% endfor %}
</div>
{% endif %}
{% endfor %}

</section>

{% endif %}
{% endfor %}


<script>
document.addEventListener('DOMContentLoaded', function () {
    const titles = document.querySelectorAll('.other-tool-title');
    titles.forEach(function (title) {
        title.addEventListener('click', function () {
            const content = title.nextElementSibling;
            content.classList.toggle('show');
            title.classList.toggle('show');
        });
    });
});
</script>
