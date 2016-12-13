# Homebrew
OS X는 기본적으로 wget명령을 내장하고 있지 않다. Apple에서 기본으로 제공하지 않는 패키지들이 필요할 때가 있을 것이다. 하나씩 다운받아 설치할 수도 있겠지만, 이를 편하게 관리할 수 있는 패키지 관리자가 바로 **Homebrew**[OS X용 패키지 관리자 설치](http://brew.sh/index_ko.html)이다.  

## Homebrew 설치하기 
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Homebrew가 하는 일은?
Homebrew는 Apple에서 제공하지 않는 `유용한 패키지 관리자`를 설치한다.
```
$ brew install wget
```

Homebrew는 전용 디렉토리에 패키지를 설치하고 /usr/local 위치로 symbolic link를 연결한다.

```
$ cd /usr/local
$ find Cellar
Cellar/wget/1.16.1
Cellar/wget/1.16.1/bin/wget
Cellar/wget/1.16.1/share/man/man1/wget.1

$ ls -l bin
bin/wget -> ../Cellar/wget/1.16.1/bin/wget
```

Homebrew는 전용 디렉토리 외부에 설치하는 일은 없지만 원하는 위치에 설치할 수도 있다.

간단하게 Homebrew 패키지를 만들어 보자.
```
$ brew create https://foo.com/bar-1.0.tgz
Created /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/bar.rb
```

모두 git과 ruby 기반으로 되어 있으며, 쉽게 수정을 되돌리거나 원본 코드의 수정사항을 병합해 봅시다. 
```
$ brew edit wget # opens in $EDITOR!
```
 
Homebrew formula는 간단한 Ruby 스크립트이다.
```
class Wget < Formula
  homepage "https://www.gnu.org/software/wget/"
  url "https://ftp.gnu.org/gnu/wget/wget-1.15.tar.gz"
  sha256 "52126be8cf1bddd7536886e74c053ad7d0ed2aa89b4b630f76785bac21695fcd"

  def install
    system "./configure", "--prefix=#{prefix}"
    system "make", "install"
  end
end
```
