<div class="index-page">

<ul>
  <%
    _.each(posts.reverse(), function(file) {
      var post = postName(file);
      var data = manifest[file];
      if (post == 'index') return;
  %>
    <li><a class="post-item" href="<%= post %>/">
      <%= data.title %>
    </a></li>
  <% }); %>
</ul>

</div>
