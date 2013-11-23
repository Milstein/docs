<!--
@title PUT categories/order
@author Moltin Ltd
@description Orders and sets the parents of a set of given cateories

@sidebar 1
@family Category
@rate No
@auth Yes
@format JSON
@http PUT
@version beta
-->

You can update the order and parents of categories in one go. This is useful if you have some sort of drag-n-drop system in place to reorder your categories.

#### Resource URL   {#resource}
PUT [{{ url }}categories/order]({{ url }}categories/order)

#### Paramaters	{#paramaters}
Key | Type | Description
--- | ---- | -----------
:id[parent] | Integer | The parent category
:id[order] | Integer | The order in which this category appears


<!--code-->
#### Example Successful Response	{#success}
``` json
{
  "status": true
}
```


### Example Failed Response	{#error}
``` json
{
  "status": false,
  "errors": []
}
```
<!--/code-->