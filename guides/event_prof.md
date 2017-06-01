# EventProf

EventProf collects instrumentation (such as ActiveSupport::Notifications) metrics during your test suite run.

It works very similar to `rspec --profile` but can track arbitrary events.

Example output:

```sh
[TEST PROF INFO] EventProf results for sql.active_record

Total time: 00:00.256
Total events: 1031

Top 5 slowest suites (by time):

AnswersController (./spec/controllers/answers_controller_spec.rb:3) – 00:00.119 (549 / 20)
QuestionsController (./spec/controllers/questions_controller_spec.rb:3) – 00:00.105 (360 / 18)
CommentsController (./spec/controllers/comments_controller_spec.rb:3) – 00:00.032 (122 / 4)

Top 5 slowest tests (by time):

destroys question (./spec/controllers/questions_controller_spec.rb:38) – 00:00.022 (29)
change comments count (./spec/controllers/comments_controller_spec.rb:7) – 00:00.011 (34)
change Votes count (./spec/shared_examples/controllers/voted_examples.rb:23) – 00:00.008 (25)
change Votes count (./spec/shared_examples/controllers/voted_examples.rb:23) – 00:00.008 (32)
fails (./spec/shared_examples/controllers/invalid_examples.rb:3) – 00:00.007 (34)

```


## Instructions

Currently, EventProf supports only ActiveSupport::Notifications and RSpec.

To activate EventProf use `EVENT_PROF` environment variable set to event name:

```sh
# Collect SQL queries stats for every suite and example
EVENT_PROF='sql.active_record' rspec ...
```

See [Rails guides](http://guides.rubyonrails.org/active_support_instrumentation.html)
for the list of available events if you're using Rails.

If you're using [rom-rb](http://rom-rb.org) you might be interested in profiling `'sql.rom'` event.

### Configuration

By default, EventProf collects information only about top-level groups (aka suites),
but you can also profile individual examples. Just set the configuration option:

```ruby
TestProf::EventProf.configure do |config|
  config.per_example = true
end
```

Another useful configuration parameter – `rank_by`. It's responsible for sorting stats – 
either by the time spent in the event or by the number of occurrences:

```sh
EVENT_PROF_RANK=count EVENT_PROF='instantiation.active_record' be rspec
```

See [event_prof.rb](https://github.com/palkan/test-prof/tree/master/lib/test_prof/event_prof.rb) for all available configuration options and their usage.