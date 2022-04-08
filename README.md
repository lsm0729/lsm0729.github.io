## 카테고리 세팅방법.

# _includes/nav_list_main

<span class..> : 대카테고리 설정
  
이후 각 for 문 마다 소카테고리 설정

'''
<span class="nav__sub-title">알고리즘</span>
            <ul>
                {% for category in site.categories %}
                    {% if category[0] == "DeepLearning" %}
                        <li><a href="/categories/DeepLearning" class="">DeepLearning ({{category[1].size}})</a></li>
                    {% endif %}
                {% endfor %}
            </ul>
'''






  
  

# _posts/대카테고리

_posts 아래에 대카테고리 이름으로 subpath 만들고 그 아래에 post 작성.

categoris 와 tags 설정
  

# _pages/categories/
  
위 path 에서 카테고리에 대한 md 생성
