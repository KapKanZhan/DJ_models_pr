 from django.contrib.auth.models import User                          
 from author.models import Author, Category, Post, Comment, PostCategory

 user1 = User.objects.create_user(username='user1')
 user2 = User.objects.create_user(username='user2')

 author1 = Author.objects.create(user=user1)
 author2 = Author.objects.create(user=user2)

 cat1 = Category.objects.create(title='Категория1')
 cat2 = Category.objects.create(title='Категория2')
 cat3 = Category.objects.create(title='Категория3')
 cat4 = Category.objects.create(title='Категория4')

 post1 = Post.objects.create(author=author1, type='A', title='Статья1', text='Текст статьи 1...')
 post2 = Post.objects.create(author=author2, type='A', title='Статья2', text='Текст статьи 2...')
 news = Post.objects.create(author=author1, type='N', title='Новость1', text='Текст новости 1...')

 post1.categories.add(cat1, cat2)           
 post2.categories.add(cat2)       
 news.categories.add(cat3, cat4)  	

 comment1 = Comment.objects.create(post=post1, user=user1, text='Комментарий 1')
 comment2 = Comment.objects.create(post=post1, user=user2, text='Комментарий 2')
 comment3 = Comment.objects.create(post=post2, user=user1, text='Комментарий 3')
comment4 = Comment.objects.create(post=news, user=user2, text='Комментарий 4')

post1.like()
post2.dislike()
news.like()
comment1.like()
comment2.dislike()
comment3.like()
comment4.like()

author1.update_rating()
author2.update_rating()

best_author = Author.objects.order_by('-rating').first()
print(best_author.user.username, best_author.rating)

user1 9

best_post = Post.objects.filter(type='A').order_by('-rating').first()
print(best_post.created_at, best_post.author.user.username, best_post.rating, best_post.title, best_post.preview())

2023-07-07 21:23:50.152096+00:00 user1 1 Статья1 Текст статьи 1......

Comment.objects.filter(post=best_post).values()

<QuerySet [{'id': 1, 'post_id': 1, 'user_id': 7, 'text': 'Комментарий 1', 'created_at': datetime.datetime(2023, 7, 7, 21, 26, 37, 872831, tzinfo=datetime.timezone.utc), 'rating': 1}, {'id': 2, 'post_id': 1, 'user_id': 8, 'text': 'Комментарий 2', 'created_at': datetime.datetime(2023, 7, 7, 21, 26, 37, 904346, tzinfo=datetime.timezone.utc), 'rating': -1}]>
