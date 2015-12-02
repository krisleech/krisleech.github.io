---
layout: post
title: "Avro in Ruby"
date: 2015-12-01 09:36
comments: true
published: true
categories: [ruby]
---

This is a short example showing the use of [Avro](https://avro.apache.org/docs/current/), a data serialization format, based on JSON.

The schema is stored in the payload along with the data. This means we turn a
Ruby Hash in to JSON and when deseriaized back to a Hash we get back the same value types as
where in the original Hash. Unlike with the Ruby Marshall format the same can happen
in other languages too.

<!-- more -->

## Writing Avro

```ruby
require 'avro'

schema = { "type": "record",
           "name": "User",
           "fields":
             [
              {"name": "name", "type": "string"},
              {"name": "points", "type": "int"},
              {"name": "winner", "type": "boolean", "default": "false"}
             ]
         }.to_json

schema = Avro::Schema.parse(schema)
writer = Avro::IO::DatumWriter.new(schema)
buffer = StringIO.new
writer = Avro::DataFile::Writer.new(buffer, writer, schema)
writer << {"name" => "Sally", "points" => 25, "winner" => true}
writer.close # important

result = buffer.string

result # => "**Obj\u0001\u0004\u0014avro.codec\bnull\u0016avro.schema\xC2\u0002{\"type\":\"r**..."
```

Avro is a binary format.

Note that `buffer` can be any IO object, e.g. a file.

## Reading Avro

```ruby
require 'avro'

buffer = StringIO.new(input)

dr = Avro::DataFile::Reader.new(buffer, Avro::IO::DatumReader.new)

data = dr.to_a # => [{ ... }]
```

The `input` is the same as `result` in the previous code segment.

You will get back a correctly typed Ruby Array of Hash.

If you want to work with Avro from the command line there is `avro-tools`, which is installable with `brew` (MacOS).

Check out [this page](https://github.com/miguno/avro-cli-examples) for some examples.
