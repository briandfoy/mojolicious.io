---
title: 'Use roles to extend Mojolicious classes'
tags:
    - roles
author: brian d foy
images:
  banner:
    src: ''
    alt: ''
    data:
      attribution: |-
        <a rel="nofollow" class="external text" href="">Image</a> by <a href=""></a> <a href="https://creativecommons.org/licenses/by-sa/2.0" title="Creative Commons Attribution-Share Alike 2.0">CC BY-SA 2.0</a>
data:
  bio: briandfoy
  description: 'Create new behavior for Mojolicious classes'
---

## Add behavior to existing classes

Sometimes a Mojolicious class doesn't do everything I should.
Sometimes that's because I'm doing it wrong, but sometimes not. When I
run into the situations where I might not be doing it wrong, I can
extend a class by applying a "role" (sometimes called a "trait" or a
"mixin") to get what I want.

---

There are times that I want
[Mojo::File](https://mojolicious.org/perldoc/Mojo/File) to act a bit
differently than it does. Often I have a path where I want to combine
only the basename with a different directory. I end up making `Mojo::File`
objects for both and then working with the directory object to get what I want:

	use Mojo::File qw(path);

	my $path     = Mojo::File->new( '/Users/brian/bin/interesting.txt' );
	my $dir      = Mojo::File->new( '/usr/local/bin' );

	my $new_path = $dir->child( $path->basename );

	say $new_path;  # /usr/local/bin/interesting.txt

That's annoying. There are a few methods that I'd like instead. I'd rather
be able to write it like this, where I start with the interesting file and keep
working on it instead of switching to some other object:

	use Mojo::File qw(path);

	my $new_path = Mojo::File
		->new( '/Users/brian/bin/interesting.txt' )
		->rebase( '/usr/local/bin' );   # this isn't a method

	say $new_path;  # /usr/local/bin/interesting.txt

I could go through various Perl tricks to add this method to
`Mojo::File` through [monkey
patching](https://mojolicious.org/perldoc/Mojo/Util#monkey_patch) or
subclassing. But, as usual, Mojolicious anticipates my desire and
provides a way to do this. I can add a role,

You can read about roles on your own while I jump into it. First, I
create a class to represent my role. I define the method(s) I want. I
use the name of the package I want to affect, add `::Role::`, then the
name I'd like to use. It's not important that its lowercase. `Mojo::Base`
sets up everything I need when I import `-role`:

	package Mojo::File::Role::rebase {
		use Mojo::Base qw(-role -signatures);

		sub rebase ($file, $dir) {
			$file->new( $dir, $file->basename )
			}
		}

I apply my new functionality by using `with_roles`. Since I used the
naming convention, I can leave off most of the package name and use
the last part of it preceded by a plus sign:

	my $file_class = Mojo::File->with_roles( '+rebase' );

Alternately I could have typed out the full package name, which I
would have to do if I didn't follow the naming convention:

	my $file_class = Mojo::File->with_roles( 'Mojo::File::Role::rebase' );

The `$file_class` is a string with the new class name. Behind that
class there is some multiple inheritance magic that you'll be much
happier ignoring. You don't need to use a bareword class name to call
class methods. A string works just as well. Now I can use my `rebase`:

	say $file_class
		->new( '/Users/brian/bin/interesting.txt' )
		->rebase( '/usr/local/bin/' );

This doesn't solve the problem of `Mojo::File` objects that I get from other
Mojolicious operations, but this is good enough for now.

