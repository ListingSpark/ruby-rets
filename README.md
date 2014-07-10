Overview
===
Simplifies the process of pulling data from RETS servers without having to worry about various authentication setups, should support all 1.x implementations. Parsing uses SAX to stream data as it comes rather than having to pull the entire document down and parse it all at once as some servers can return quite a lot of data.

Compability
-
Tested against Ruby 1.8.7, 1.9.2, 2.0.0, RBX and JRuby, build history is available [here](http://travis-ci.org/Placester/ruby-rets).

<img src="https://secure.travis-ci.org/Placester/ruby-rets.png?branch=master&.png"/>

Documentation
-
See http://rubydoc.info/github/Placester/ruby-rets/master/frames for full documentation.

Examples
-

    client = RETS::Client.login(:url => "http://foobar.com/rets/Login", :username => "foo", :password => "bar")
    client.search(:search_type => :Property, :class => :RES, :query => "(ListPrice=50000-)") do |data|
      # RETS data in key/value format, as COMPACT-DECODED
    end

    client.get_object(:resource => :Property, :type => :Photo, :location => false, :id => "1:0:*") do |headers, content|
      puts "Object-ID #{headers"object-id"]}, Content-ID #{headers["content-id"]}, Description #{["description"]}"
      puts "Data"
      puts content
    end

VCR / WebMock
-
Due to the streaming parser, the search features won't work with a library like VCR or Ephemeral Response. For WebMock, you can use the below patch to enable support for saving the HTTP requests to speed up your own tests.

    module Net
        module WebMockHTTPResponse
            def self.extended(response)
                response.instance_variable_set(:@socket, StringIO.new(response.body))
            end
        end
    end

License
-
Licensed under MIT
