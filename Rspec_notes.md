#Philosophy fo Test
- Tests should be reliable
- Tests should be easy to write
- Tests should be easy to understand

##Test for model
To get started, a model spec should include tests for the following:
- The model’s create method, when passed valid attributes, should be valid.
- Data that fail validations should not be valid.- Class and instance methods perform as expected.

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
describe Contact do	it "is valid with a firstname, lastname and email" do      contact = Contact.new(        firstname: 'Aaron',        lastname: 'Sumner'        email: 'tester@example.com')		expect(contact).to be_valid
	end
end
```

- **Remember change `to` to `to_not`  to test if the spec works correctly**
- **don't just test the valid cases but also test the invalid cases**




严格来说,describe 和 context 是可以互换的,但我倾向于这么用: describe 用来表示需要实现的功能,而 context 针对该功能不同的情况

- 明确指定希望得到的结果:使用动词说明希望得到结果。每个测试用例只测试一 种情况。- 测试希望看到的结果和不希望看到的结果:测试时要分别思考两种情况,然后分 别测试。- 测试极端情况:如果有一个数据验证要求密码的长度在 4∼10 个字符范围内,不 要只测试 8 个字符的密码就觉得可以了。一个合格的测试应该分别检测 4 个字符 和 10 个字符的情况,还要检测 3 个字符和 11 个字符的情况。(当然,借由这个机 会你也可以思考一下为什么我要把密码限制的这么短,再长一些会不会更好。测 试的过程中往往会有很多灵感,可以优化程序的需求和代码。)- 测试要良好的组织,保证可读性:使用describe和context组􏰁测试,形成一个 清晰的大纲;使用 before 和 after 块消除代码重复。不过,当可读性更重要时, 例如避免总是来来回回的查看测试文件,就可以适当的有点重复。

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