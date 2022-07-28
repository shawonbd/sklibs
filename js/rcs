function innerTotalComments(json) {
  var total = parseInt(json.feed.openSearch$totalResults.$t);
  var seen = parseInt(localStorage.getItem('seen'));
  seen = seen ? seen : 0;
  document.querySelector('.more-cmt').innerHTML = `See all <b>${total}</b> Comments`;
  document.querySelector('.more-cmt').title = `See all ${total} Comments`;
  if (total > seen) {
    document.querySelector('.popup-cmt').dataset.text = total - seen;
  } else if (total < seen) {
    localStorage.setItem('seen', total);
  }
}

function innerComment() {
  var e = "<ul class='cmt-ul'>";
  for (let i = 0; i < limCmt; i++) {
    var comment = comments[i];
    e += `
      <li class="cmt-li">
        <div class="cmt-info">
          <img class="cmt-avatar" src=${comment.avatar} />    
        </div>
        <a class="cmt-content" href=${comment.link} rel="nofollow" title="${comment.content}">
          <p>${comment.content}</p>
          <span>${comment.author} - ${comment.time}</span>
        </a>
      </li>
    `;
  }
  e += '</ul>';
  document.querySelector('.cmt-list').innerHTML = e;
}

function rcComments(json) {
  for (let i = 0; i < limCmt; i++) {
    var commentJson = json.feed.entry[i];
    var name = commentJson.author[0].name.$t;
    name.length < lengthName ? (name = name) : (name = name.substring(0, lengthName) + '&#133;');
    var content = commentJson.content.$t;
    content = content.replace(/(<([^>]+)>)/g, '');
    content.length < lengthContent ? (content = content) : (content = content.substring(0, lengthContent) + '&#133');
    var published = new Date(commentJson.published.$t);
    var now = new Date();
    var second = Math.floor((now - published) / 1000);
    var time =
      second < 60
        ? second + ' seconds ago'
        : second < 3600
        ? Math.floor(second / 60) + ' minutes ago'
        : second < 60 * 60 * 24
        ? Math.floor(second / (60 * 60)) + ' hours ago'
        : second < 60 * 60 * 24 * 7
        ? Math.floor(second / (60 * 60 * 24)) + ' weeks'
        : Math.floor(second / (60 * 60 * 24 * 7)) + ' months';
    var comment = {
      author: name,
      avatar:
        commentJson.author[0].gd$image.src == 'https://img1.blogblog.com/img/b16-rounded.gif'
          ? noAvatar
          : commentJson.author[0].gd$image.src == 'https://img1.blogblog.com/img/blank.gif'
          ? noAvatar
          : commentJson.author[0].gd$image.src,
      content: content,
      time: time,
      link: commentJson.link[2].href,
    };
    comments.push(comment);
  }
  innerComment();
  innerTotalComments(json);
}

document.write(
  `<script type="text/javascript" src="${blogUri}/feeds/comments/default?alt=json-in-script&max-results=${limCmt}&callback=rcComments"></script>`
);

var commentBtn = document.querySelector('.popup-cmt');
commentBtn.addEventListener('click', function () {
  var newCmt = parseInt(commentBtn.dataset.text);
  var seen = parseInt(localStorage.getItem('seen'));
  seen = seen ? seen : 0;
  if (newCmt) {
    localStorage.setItem('seen', newCmt + seen);
    delete commentBtn.dataset.text;
  }
});
