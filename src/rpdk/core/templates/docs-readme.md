{% autoescape false %}
# {{ type_name }}
{% if schema.description %}

{{ schema.description }}
{% endif %}

## Syntax

To declare this entity in your AWS CloudFormation template, use the following syntax:

### JSON

<pre>
{
    "Type" : "{{ type_name }}",
    "Properties" : {
{% if schema.properties %}
{% for propname, prop in schema.properties.items() %}
{% if not prop.readonly %}
        "<a href="#{{ propname.lower() }}" title="{{ propname }}">{{ propname }}</a>" : <i>{{ prop.jsontype }}</i>{% if not loop.last %},{% endif %}

{% endif %}
{% endfor %}
{% endif %}
    }
}
</pre>

### YAML

<pre>
Type: {{ type_name }}
Properties:
{% if schema.properties %}
{% for propname, prop in schema.properties.items() %}
{% if not prop.readonly %}
    <a href="#{{ propname.lower() }}" title="{{ propname }}">{{ propname }}</a>: <i>{{ prop.yamltype }}</i>
{% endif %}
{% endfor %}
{% endif %}
</pre>
{% if schema.properties %}

## Properties

{% for propname, prop in schema.properties.items() %}
{% if not prop.readonly %}
#### {{ propname }}
{% if prop.description %}

{{ prop.description }}
{% endif %}

{% if schema.required is defined and propname in schema.required %}
_Required_: Yes
{% else %}
_Required_: No
{% endif %}

_Type_: {{ prop.longformtype }}
{% if prop.allowedvalues %}

_Allowed Values_: {% for allowedvalue in prop.allowedvalues %}<code>{{ allowedvalue }}</code>{% if not loop.last %} | {% endif %}{% endfor %}

{% endif %}
{% if prop.minLength %}

_Minimum Length_: <code>{{ prop.minLength }}</code>
{% endif %}
{% if prop.maxLength %}

_Maximum Length_: <code>{{ prop.maxLength }}</code>
{% endif %}
{% if prop.pattern %}

_Pattern_: <code>{{ prop.pattern }}</code>
{% endif %}
{% if prop.createonly %}

_Update requires_: [Replacement](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-replacement)
{% elif prop.conditionalCreateOnly %}

_Update requires_: [Some interruptions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-some-interrupt)
{% else %}

_Update requires_: [No interruption](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html#update-no-interrupt)
{% endif %}

{% endif %}
{% endfor %}
{% endif %}
{% if getatt or ref %}
## Return Values
{% if ref %}

### Ref

When you pass the logical ID of this resource to the intrinsic `Ref` function, Ref returns the {{ ref }}.
{% endif %}
{% if getatt %}

### Fn::GetAtt

The `Fn::GetAtt` intrinsic function returns a value for a specified attribute of this type. The following are the available attributes and sample return values.

For more information about using the `Fn::GetAtt` intrinsic function, see [Fn::GetAtt](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getatt.html).

{% for att in getatt %}
#### {{ att.name }}
{% if att.description %}

{{ att.description }}
{% endif %}

{% endfor %}
{% endif %}
{% endif %}
{% endautoescape %}
