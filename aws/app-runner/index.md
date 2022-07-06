# AWS App Runner Journey

[AWS App Runner](https://aws.amazon.com/apprunner), is a fully managed service that makes it easy for developers to quickly
deploy containerized web applications and APIs, at scale and with no prior infrastructure experience required

> Unfortunately, as of today July 3, 2022. App Runner only supports certain regions. Which makes usage limited especially when
> we have to set up networking in the same VPC as where all our current AWS components are (ap-southeast-1 - Singapore)
>
> Can revisit this at a later date when App Runner is in more regions (specifically ap-southeast-1 - Singapore)

## Notes

From [Drift Ruby - App Runner video tutorial](https://www.driftingruby.com/episodes/aws-app-runner)

### Pushing to ECR:

```shell
docker buildx build -f Dockerfile.staging --platform linux/amd64 -t 316537114178.dkr.ecr.ap-northeast-1.amazonaws.com/api:latest --push ./.
```

### In `Dockerfile.staging`:

> Needs a cleanup

```shell
FROM --platform=linux/amd64 ruby:<ruby-verion-here>

# Remove /var/cache/apk/* to keep the image light
RUN apt-get update -qq \
  && apt-get install -y nodejs postgresql-client vim \
  && rm -rf /var/cache/apk/*

# RUN apt-get install ca-certificates

# RUN apk add --no-cache --update build-base \
#   git \
#   nodejs \
#   yarn \
#   imagemagick \
#   tzdata \
#   && rm -rf /var/cache/apk/*

COPY Gemfile Gemfile.lock ./
# This was the cause:
# RUN gem update --system
# RUN gem install bundler

# Pass -j $(nproc) to use as many cores as available
# in the system
# RUN bundle install -j $(nproc)

RUN bundle config --local gems.contribsys.com <actual-username-here>:<actual-password-here>

WORKDIR /api
COPY . /api

RUN sed -i 's/mozilla\/DST_Root_CA_X3.crt/!mozilla\/DST_Root_CA_X3.crt/' /etc/ca-certificates.conf
RUN update-ca-certificates

# Pass -j $(nproc) to use as many cores as available
RUN bundle install -j $(nproc)
# RUN yarn install

# WORKDIR /app
ENV RAILS_ENV=staging

# These env vars are for config/environments/production.rb
ENV RAILS_LOG_TO_STDOUT=true
ENV RAILS_SERVE_STATIC_FILES=true

ENV PORT=3000
# ENV PORT=8080

# Copy the rest of the files
# COPY . .

RUN rake assets:precompile
# This line will work in updated versions of Rails
# RUN bin/rails assets:precompile

# Unfortunately, currently with AWS App Runner
# There is no console to log into to access your runnning
# applications. Entrypoint.sh will be responsible for running
# migration on every deployment automatically
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT [ "entrypoint.sh" ]

EXPOSE 3000
# EXPOSE 8080

CMD ["rails", "server", "-b", "0.0.0.0"]
```

### Enable VPC communication

To enable your services to communicate with databases and other applications hosted in an Amazon Virtual Private Cloud

Referenced at [App Runner VPC support](https://aws.amazon.com/blogs/aws/new-for-app-runner-vpc-support)

Create a role called `AWSAppRunner_StagingRole`

```shell
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Service": "build.apprunner.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
    ]
  }
```

Attach `AWSAppRunnerServicePolicyForECRAccess` to this new role

Then create a new policy called `AWSAppRunner_RoleAccess_Staging` and attach to the above role

```shell
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "rds-db:connect"
        ],
        "Resource": [
          "arn:aws:rds-db:ap-southeast-1:316537114178:dbuser:<db-user>/<app>"
        ]
      }
    ]
  }
```

## References

- [Drift Ruby - App Runner video tutorial](https://www.driftingruby.com/episodes/aws-app-runner) - Requires a paid subscription
- [App Runner VPC support](https://aws.amazon.com/blogs/aws/new-for-app-runner-vpc-support)
- [How App Runner works with IAM](https://docs.aws.amazon.com/apprunner/latest/dg/security_iam_service-with-iam.html#security_iam_service-with-iam-roles)
- [Creating and using an IAM policy for IAM database access](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.IAMPolicy.html)
