---
title: "나도 몰라 적어두는 노트"
keywords: home
tags: [Home]
sidebar: mydoc_sidebar
permalink: index.html
summary: 이 노트는 작성자 본인을 위한 것으로서 내용이 다소 자세하지 않을 수 있습니다.
---

<script src="js/jquery.shuffle.min.js"></script>
<script src="js/jquery.ba-throttle-debounce.min.js"></script>

      <div class="filter-options">
      <button class="btn btn-primary" data-group="all">All</button>
      <button class="btn btn-primary" data-group="home">Home</button>
      <button class="btn btn-primary" data-group="web">Web</button>
      <button class="btn btn-primary" data-group="tools">Tools</button>
    </div>      

<div id="grid" class="row">


    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["home"]'>

               <div class="panel panel-default">
               <div class="panel-heading">Home</div>
               <div class="panel-body">
                  <ul>
                {% for page in site.pages %}
                {% for tag in page.tags %}
                {% if tag == "Home" %}
                  <li><a href="{{page.url | remove: '/'}}">{{page.title}}</a></li>
                {% endif %}
                {% endfor %}
                {% endfor %} 
                  </ul>
               </div>
            </div>
    
    </div>
   

    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["web"]'>

        <div class="panel panel-default">
            <div class="panel-heading">Web</div>
            <div class="panel-body">
                <ul>
                    {% for page in site.pages %}
                    {% for tag in page.tags %}
                    {% if tag == "Web" %}
                    <li><a href="{{page.url | remove: '/'}}">{{page.title}}</a></li>
                    {% endif %}
                    {% endfor %}
                    {% endfor %}
                </ul>
            </div>
        </div>
        
    </div>



    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["tools"]'>

                <div class="panel panel-default">
               <div class="panel-heading">Tools</div>
               <div class="panel-body">
                  <ul>
                {% for page in site.pages %}
                {% for tag in page.tags %}
                {% if tag == "Tools" %}
                  <li><a href="{{page.url | remove: '/'}}">{{page.title}}</a></li>
                {% endif %}
                {% endfor %}
                {% endfor %}
                  </ul>
               </div>
            </div>

    </div>
    <!-- sizer -->
    <div class="col-xs-6 col-sm-4 col-md-1 shuffle_sizer"></div>          


    </div><!-- /#grid -->


{% include initialize_shuffle.html %}