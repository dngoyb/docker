FROM ruby

RUN apt-get update -y && apt-get upgrade -y \
	&& apt-get clean -y && apt-get autoremove -y \
	&& apt-get install nodejs -y \
	&& gem install rails bundler

ONBUILD WORKDIR /opt

ONBUILD RUN rails new app
ONBUILD WORKDIR /opt/app
ONBUILD RUN bundle install \
			&& rails generate migration CreateProducts name:string \
			&& rake db:migrate
