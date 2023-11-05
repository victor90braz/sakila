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

----------- GROUP BY ---------------------
SELECT users.email, users.`name`, users.remember_token COUNT(comments.post_id)
FROM users
LEFT JOIN `comments` ON `comments`.post_id = post_id
GROUP BY users.email, users.`name`, users.remember_token;

# RELATIONSHIPS

# ONE-TO-ONE

-----------CREATE DB RELATIONSHIP ONE-TO-ONE ---------------------

SELECT \* from users
join profiles
on profiles.user_id = user_id

# SCHEMA

public function up(): void
{
Schema::create('profiles', function (Blueprint $table) {
$table->bigIncrements('id');
$table->unsignedBigInteger('user_id');
$table->string('website');
$table->string('github');
$table->string('twitter');
$table->timestamps();

        $table->foreign('user_id')->references('id')->on('users')
        ->onDelete('cascade');
    });

}

----------- RELATIONSHIP ONE-TO-ONE ---------------------

# ONE-TO-ONE STEPS

    class User extends Authenticatable
    {
        use HasFactory;

        public function profile() {
            return $this->belongsTo(Profile::class);
        }
    }

    class Profile extends Model
    {
        use HasFactory;
        protected $guarded = [];

        public function user() {
            return $this->belongsTo(User::class);
        }
    }

    note: protected $guarded = []; // remember to add

# tinker

php artisan tinker
$user = new User
$user->first()
$user->first()->profile

#
