---
title: "비공개 노트"
keywords: home
tags: [private_home]
sidebar: private_sidebar
permalink: private_index.html
summary: 이 노트는 대외비로서 외부에 유출되면 안되는 비공개 노트입니다.
---

<script src="js/jquery.shuffle.min.js"></script>
<script src="js/jquery.ba-throttle-debounce.min.js"></script>

<div class="filter-options">
  <button class="btn btn-primary" data-group="all">All</button>
  <button class="btn btn-primary" data-group="private_home">Home</button>
  <button class="btn btn-primary" data-group="samsung">SAMSUNG</button>
</div>      

<div id="grid" class="row">
    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["private_home"]'>

               <div class="panel panel-default">
               <div class="panel-heading">Home</div>
               <div class="panel-body">
                  <ul>
                {% for page in site.pages %}
                {% for tag in page.tags %}
                {% if tag == "private_home" %}
                  <li><a href="{{page.url | remove: '/'}}">{{page.title}}</a></li>
                {% endif %}
                {% endfor %}
                {% endfor %} 
                  </ul>
               </div>
            </div>
    </div>
   

    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["samsung"]'>

        <div class="panel panel-default">
            <div class="panel-heading">SAMSUNG</div>
            <div class="panel-body">
                <ul>
                    {% for page in site.pages %}
                    {% for tag in page.tags %}
                    {% if tag == "samsung" %}
                    <li><a href="{{page.url | remove: '/'}}">{{page.title}}</a></li>
                    {% endif %}
                    {% endfor %}
                    {% endfor %}
                </ul>
            </div>
        </div>
        
    </div>
    </div><!-- /#grid -->


{% include initialize_shuffle.html %}