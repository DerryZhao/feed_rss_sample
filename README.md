Welcome to the feed_rss_sample wiki!
```ruby
require 'rss'
require 'open-uri'

class Feed

	#获取feed资源
	def method_missing(meth, *args, &blk)
		url = 'http://money.163.com/special/00252EQ2/toutiaorss.xml'
		open(url) do |rss|
		  feed = RSS::Parser.parse(rss)
		  puts "Title: #{feed.channel.title}"
		  feed.items.each do |item|
		    puts "Item: #{feed_patten(item)}"
		  end
		end
		true
	end
	#解析rss
	def feed_patten(item)
		{
			title: item.title,
			author: item.author,
			category: item.category,
			source: item.source,
			link: item.link,
			description: item.description,
			pubdate: item.pubDate,
			guid: item.guid.content,
			comments: item.comments
		}
	end
	#生成rss源
	def make_rss
		rss = RSS::Maker.make("atom") do |maker|
		  maker.channel.author = "matz"
		  maker.channel.updated = Time.now.to_s
		  maker.channel.about = "http://www.sohaobao.com"
		  maker.channel.title = "好博文 Feed"

		  maker.items.new_item do |item|
		    item.link = "http://www.sohaobao.com"
		    item.title = "好博文"
		    item.updated = Time.now.to_s
		    item.description = "好博文好博文好博文好博文好博文好博文好博文好博文"
		  end
		end
		rss
	end

end`