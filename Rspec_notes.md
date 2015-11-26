#Philosophy fo Test
- Tests should be reliable
- Tests should be easy to write
- Tests should be easy to understand

##Test for model
To get started, a model spec should include tests for the following:

- Data that fail validations should not be valid.

```ruby
describe Contact do
  it "is valid with a firstname, lastname and email"
  it "is invalid without a firstname"
  it "is invalid without a lastname"
  it "is invalid without an email address"
  it "is invalid with a duplicate email address"
  it "returns a contact's full name as a string"
end
```

- This spec describe a model Contact and it behavior
- use verb as the begining of a sentence
- each `it` represent a `expectation`


```ruby
describe Contact do
	end
end
```

- **Remember change `to` to `to_not`  to test if the spec works correctly**
- **don't just test the valid cases but also test the invalid cases**




严格来说,describe 和 context 是可以互换的,但我倾向于这么用: describe 用来表示需要实现的功能,而 context 针对该功能不同的情况

- 明确指定希望得到的结果:使用动词说明希望得到结果。每个测试用例只测试一 种情况。

##利用factory girl生成test data

- Factory Girl 是用來生成測試數據的gem
- Factory Girl, an easy-to-use and easy-to-rely-on gem for creating test data without the brittleness of fixtures.


```ruby
#In spec/factories/contacts.rb
FactoryGirl.define do
  factory :contact do
    firstname "John"
    lastname "Doe"
    sequence(:email) {|n| "johndoe#{n}@example.com"}
   end
end

```

```ruby
#In spec/model/contact_spec.rb

describe Contact do
	it "......" do
		FactoryGirl.build(:contact)
	
	end
end
```

##Better reliable slug mechanism