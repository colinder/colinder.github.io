# Django_07_1:N 불러오기


​	

# Django 1:N 불러오기

> **prefetch_related()** or **annotate()**

​		

models.py 구성

```python
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
```

```python
class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE, related_name='choices')
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

​					

​				

### prefetch_related()

```python
# views.py
def get_context_data():
    context["questions"]  = Question.objects.prefetch_related('choices')
    return context

# templates/.html
{% for question in questions %}
    <b>{{ question.question_text }}</b>
    <ul>
        {% for choice in question.choices.all %}
            <li>{{ choice.choice_text }}</li>
        {% endfor %}
    </ul>
{% endfor %}

# Choice 모델에서 related_name을 지정하지 않았을 경우에는 템플릿 쪽에서 question.choice_set.all 을 사용하면 된다.
```

​			

​		

### annotate()

```python
def get_context_data():
	question_data_with_choices = Question.objects.annotate(choice_id=F('choices__id'), choice_text=F('choices__choice_text'), votes=F('choices__votes')).annotate(Count('id))
```

​	

​	

​	

​	

