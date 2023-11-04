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

# join

SELECT \* FROM `posts` JOIN `comments` ON `comments`.`post_id` = `posts`.`id`;
