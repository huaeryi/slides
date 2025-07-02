.PHONY: live build clean

live:
	@echo "Previewing working slides..."
	-@reveal-md working.md -w  --template template.html || true

build:
	@echo "Building working slides..."
	@reveal-md working.md --template template.html --static ../site --assets-dir assets
	@rm ../site/working.html

main:
	@echo "Previewing main slides..."
	-@reveal-md working.md -w  --template template.html || true

clean:
	@echo "Cleaning up..."
	rm -rf ../site/