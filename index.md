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
  <button class="btn btn-primary" data-group="big data & machine learning">Big Data & Machine Learning</button>
  <button class="btn btn-primary" data-group="tools">TDD</button>
  <button class="btn btn-primary" data-group="tools">Tools</button>
  <button class="btn btn-primary" data-group="troubleshooting">Troubleshooting</button>
  <button class="btn btn-primary" data-group="web">Web</button>
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
   
    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["big data & machine learning"]'>

               <div class="panel panel-default">
               <div class="panel-heading">Big Data & Machine Learning</div>
               <div class="panel-body">
                  <ul>
                {% for page in site.pages %}
                {% for tag in page.tags %}
                {% if tag == "Big Data & Machine Learning" %}
                  <li><a href="{{page.url | remove: '/'}}">{{page.title}}</a></li>
                {% endif %}
                {% endfor %}
                {% endfor %} 
                  </ul>
               </div>
            </div>
    </div>    

    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["TDD"]'>

               <div class="panel panel-default">
               <div class="panel-heading">TDD</div>
               <div class="panel-body">
                  <ul>
                {% for page in site.pages %}
                {% for tag in page.tags %}
                {% if tag == "TDD" %}
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

    <div class="col-xs-6 col-sm-4 col-md-4" data-groups='["troubleshooting"]'>

               <div class="panel panel-default">
               <div class="panel-heading">Troubleshooting</div>
               <div class="panel-body">
                  <ul>
                {% for page in site.pages %}
                {% for tag in page.tags %}
                {% if tag == "Troubleshooting" %}
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


    </div><!-- /#grid -->


{% include initialize_shuffle.html %}