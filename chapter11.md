# 11章 一度に1つのことを

## 問題（中ボス）

「ソースコード1」内のメソッドに含まれる機能を箇条書きで列挙してみましょう

### ソースコード 1

```ruby
#
# 書籍の zip ファイルをimportする
#
def import_zip(zip_path)
  FileUtils.mkdir_p(ZIP_CONTENTS_PATH)

  # zip を解凍して解凍先のパスを取得する
  unzip_dir = ZIP_CONTENTS_PATH.join(File.basename(zip_path, '.zip'))
  system("/usr/bin/unzip -o #{zip_path} -d #{unzip_dir} > /dev/null")
  entry_dir = Dir.glob(unzip_dir.join('*')).detect do |path|
    !path.include?('__MACOSX')
  end

  project_xml_path = File.join(entry_dir, 'project.xml')

  # project.xml取り込み
  xml = Nokogiri::XML.parse(
    File.open(project_xml_path) {|project_xml| project_xml.read }
  )
  project_info = get_project_info(xml)

  if project_info[:book][:isbn].blank?
    raise 'isbn not found in project.xml'
  end

  ActiveRecord::Base.transaction do
    book =  Book.find_or_initialize_by(isbn: project_info[:book][:isbn])
    book.attributes = project_info[:book]
    book.save!

    prelist = book.book_entries
    addlist = []

    # HTML(本文)データ取り込み
    Dir.glob(File.join(entry_dir, '*.html')).each do |html_file|
      # chapter_number, section_number, subsection_number を取得
      chapter_number, section_number, subsection_number = ...

      # bodyのsrcを置き換える
      html = File.open(html_file) {|f| f.read }
      doc = Nokogiri::HTML.parse(html)

      # 既存のブックエントリを取得する。ない場合は新規作成する
      book_entry = book.book_entries.find_or_initialize_by(
        chapter_number: chapter_number,
        ...
      )

      # ブックエントリに読み込んだメタデータを上書きする
      book_entry.attributes = {
        chapter_title: chapter_info[:chapter_title],
        ...
      }
      book_entry.save!
      addlist.push(book_entry.id)
      book_entry.reference_images.destroy_all

      # HTML内のimg要素の変換
      doc.search('img').each do |img|
        # 画像データ取り込み
        ...
        image = File.open(new_name, 'r') {|f| Image.create(image: f) }
        img['src'] =  image.decorate.image_url
      end

      book_entry.update!(body: doc.at('//article').to_s)
    end

    # 存在しない章節を削除する
    prelist.where.not(id: addlist).destroy_all
  end

  # 後始末
  FileUtils.remove_entry_secure(unzip_dir)
end
```
