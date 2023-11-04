alter table `comments`
add FOREIGN key (post_id) REFERENCES posts(id)

alter table `comments`
add FOREIGN key (post_id) REFERENCES post_id(id) on DELETE CASCADE

alter table `comments`
add FOREIGN key (post_id) REFERENCES post_id(id) on DELETE RESTRICT

alter table `comments`
add FOREIGN key (post_id) REFERENCES post_id(id) on DELETE no ACTION

alter table `comments`
add FOREIGN key (post_id) REFERENCES post_id(id) on DELETE set NULL

# FOREIGN

        Schema::create('comments', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->text('body');
            $table->unsignedBigInteger('post_id');
            $table->unsignedBigInteger('user_id');
            $table->timestamps();

            $table->foreign('post_id')->references('id')
            ->on('posts')->onDelete('cascade');
        });

# join = get all queries where they match

SELECT \* FROM `posts` JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;
SELECT \* FROM `posts` inner JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;
SELECT \* FROM `posts` left JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;
SELECT \* FROM `posts` right OUTER JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;

# left OUTER JOIN = get all queries from posts

# right OUTER JOIN = get all queries from comments

----------- notes ---------------------

SELECT \* from users

or

SELECT
users.email, users.`name`, users.remember_token
from users

----------- notes ---------------------

SELECT users.email, users.`name`, users.remember_token
from users
left JOIN `comments` on `comments`.post_id = post_id
